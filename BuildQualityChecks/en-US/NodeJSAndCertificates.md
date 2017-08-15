[Back to Overview](./overview.md)

# Issue "Unable to Verify the First Certificate"
Version 2.4.x of the *Build Quality Checks* extension does not work on a Team Foundation Server configured with a self-signed or corporate
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
For now the only workaround is to set the variable *NODE_TLS_REJECT_UNAUTHORIZED* to zero on each build machine. If you want to make sure that
this change does not affect node processes other than the ones started by our build agent, please set the variable in the context of the build
service account. We know that this solution is far from perfect, but it effectively uses the same mechanism as the older Node API and there is no
way for custom extensions using the vsts-node-api to work around the issue.

### Long-Term Solution
Starting with version 7.3.0 NodeJS introduced a new environment variable called *NODE_EXTRA_CA_CERTS* (see [here](https://nodejs.org/dist/latest-v8.x/docs/api/cli.html#cli_node_extra_ca_certs_file)),
which adds support for additional root certificates. There is no need to write custom code to use this. However, even the newest build agents
are using a pre-7.3.0 version of NodeJS and the agent team cannot simply swap in a different version of NodeJS. In the future the team will provide an updated
agent version, which supports NodeJS 7.x or 8.x, but there is no specific timeframe for this change yet.