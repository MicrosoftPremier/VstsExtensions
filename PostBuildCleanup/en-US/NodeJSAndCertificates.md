[Back to Overview](./overview.md)

# Issue "Unable to Verify the First Certificate"
Version 2.0.x of the *Post Build Cleanup* extension does not by default work on a Team Foundation Server configured with a self-signed or corporate
SSL certificate. When the build task executes, you will see the error message **unable to verify the first certificate** in the task log.
This issue is caused by the way NodeJS validates server certificates on https connections. For some background please read the following
sections or just skip to the [Workaround](#workaround) section for information about how to fix the issue.

### NodeJS and Certificate Validation
NodeJS comes with a pre-compiled, hardcoded set of trusted root certificates and does not look at a Windows machine’s certificate store or
the equivalent certificate folders/stores on other operating systems. Thus, NodeJS cannot validate a server certificate if it has not been
issued by one of the included root certificate authorities (CA), even if you install the self-signed certificate or the corporate root CA
certificate on the machine. Up to NodeJS 7.3.0 there is no easy way to extend those root certificates other than writing custom code to
include some arbitrary certificate file.

### What is the Root Cause of the Issue?
Version 2.4.x uses the latest version of the Visual Studio Team Services Node API (vsts-node-api v6.2.7-preview). In previous versions our Node API team
internally set the environment variable *NODE_TLS_REJECT_UNAUTHORIZED* to zero. This effectively allowed NodeJS to connect to https endpoints,
even when the certificate chain was broken. As this was a potential security issue, the team removed the variable in the newer versions of the API.
While this change increased security, it creates a problem for servers that use either self-signed or corporate SSL certificates, because neither
the self-signed nor the corporate root CA certificate is present in Node's list of trusted root certificates.

### Workaround
In version 2.1.0 we have added the option *Disable NodeJS certificate check* to the *Advanced* parameter section of the task. In essence the option
sets the variable *NODE_TLS_REJECT_UNAUTHORIZED* to zero for the NodeJS process used by our task. While this solution is far from perfect, it effectively
uses the same mechanism that the older versions of the Visual Studio Team Services Node API used. For now there is no better way for our task to fix
the issue.

### What is the Risk of the Workaround?
The risk of this workaround is fairly small. First of all, certificate checks are only disabled within the *Post Build Cleanup* task if you enable
the workaround. We guarantee that we only communicate with your Team Foundation Server. However, if you want to inspect our code, just download our
extension from the Visual Studio Marketplace or contact us for a copy of the TypeScript code. If your build machines are not connected to the internet,
the risk is futher mitigated. Lastly, NodeJS has no truely secure way of handling these situations. Every NodeJS script may disable the certificate
check and you cannot fully prevent this. Neither the custom code solution mentioned above nor the new environment variable in NodeJS (see below) run
any security checks on the injected certificates. Unless NodeJS implements mechanisms to work with secured certificate stores on the various operating
systems, you need to be aware of this issue and inspect all NodeJS processes running in sensitive environment for security issues.

### Long-Term Solution
Starting with version 7.3.0 NodeJS introduced a new environment variable called *NODE_EXTRA_CA_CERTS* (see [here](https://nodejs.org/dist/latest-v8.x/docs/api/cli.html#cli_node_extra_ca_certs_file)),
which adds support for additional root certificates. There is no need to write custom code to use this. However, even the newest build agents
are using a pre-7.3.0 version of NodeJS and the agent team cannot simply swap in a different version of NodeJS. In the future the team will provide an updated
agent version, which supports NodeJS 7.x or 8.x, but there is no specific timeframe for this change yet.

**Update (v2.1.0):** Even though the aforementioned variable *NODE_EXTRA_CA_CERTS* is not documented until NodeJS 7.3.0 it was actually
first implemented in version 6.10.0. Thus, we highly recommend using this variable instead of disabling the NodeJS certificate check in the
task. We have added a new warning to the *Post Build Cleanup* task that appears if you have disabled certificate checks on an agent that
supports the new variable. __*We would like to specially thank Jonathan B. for reporting that the variable worked!*__