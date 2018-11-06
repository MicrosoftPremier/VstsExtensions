[Known Issues](#known-issues) | [Support](#support) | [Task Parameters](#task-parameters)

# Create Work Item
The *Create Work Item* task allows you to create a work item from a build or release.

### Change Notes
You can find the changes notes for this task [here](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/CreateWorkItem/en-US/changeNotes.md).

### Known Issues
- Due to an issue in extension handling of Team Foundation Server and Azure DevOps Services (and a complementary mistake in our extension manifests) you might get the message "Error finding the extension" when you try to open one of the custom task editors using the "three dots" buttons. If you run into this issue, please contact us via email so we can work with you to fix the problem.

### Support
If you need help with the extension, run into issues, or have feedback or ideas for new features, please contact us at <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a> or create an issue [here](https://github.com/MicrosoftPremier/VstsExtensions/issues).

### Task Parameters
The task supports the default parameters listed below. All parameters support variables *including nested variables*.

![Default Task Parameters](../assets/DefaultInputs.png "Configuring the Create Work Item task")

- <a name="workItemType">**Work Item Type:**</a> Specify the work item type you want to create. If you know the exact name of the work item type, simply type it into the textbox. You may also click on the selection icon on the right (three dots) and select a work item type from the drop-down list displayed in the opening dialog. This field is required.

  ![Work Item Type Picker](../assets/WorkItemTypePicker.png "Selecting a work item type")

  **Note:** The dialog only shows work item types that you can create using the regular Azure DevOps UI. While technically possible, we do not recommend creating hidden work item types like code review work items.

- <a name="title">**Title:**</a> Enter the title for the new work item. This field is required.

- <a name="assignedTo">**Assigned To:**</a> Optionally, enter an identity to assign the new work item to. Since there currently is no way to use the regular identity picker control in build tasks or extensions, you have to manually enter the value in the correct format. Please make sure to use the format **Display Name &lt;Login Email Address&gt;** (e.g., *John Doe &lt;johnd@contoso.com&gt;*). If you use an incorrect format, the task will fail to create the work item.

- <a name="areaPath">**Area Path:**</a> Enter an optional area path for the new work item. If you omit this value, the work item will automatically be created in the default area path (i.e., the root area unless you created a special work item rule). Click the selection icon on the right (three dots) to select one of the existing areas.

  ![Area Path Picker](../assets/AreaPathPicker.png "Selecting an area path")

- <a name="iterationPath">**Iteration Path:**</a> Enter an optional iteration path for the new work item. If you omit this value, the work item will automatically be created in the default iteration path (i.e., the root iteration unless you created a special work item rule). By clicking the selection icon on the right (three dots) you can open a special iteration path picker that lets you choose between a specific iteration path (fixed value for all new work item) or a team name. If you select the team option, the new work item will always be created in the team's current iteration. In case there is no current iteration (e.g., no iterations have been associated with the team) the task falls back to the team's default iteration or the team's backlog iteration in that order depending on what value is available.

  ![Iteration Path Picker - Specific](../assets/SpecificIterationPathPicker.png "Selecting a specific iteration path") ![Iteration Path Picker - Team](../assets/TeamIterationPathPicker.png "Selecting a team iteration path")

  **Note:** When you select a team's current iteration, the iteration path parameter has the special value *Team ID;@currentIteration*. Do not change this value manually or the work item creation might fail.

- <a name="fieldMappings">**Additional Fields:**</a> If you want to set additional field values, specify them here. Each line in the text box should contain a field-value-mapping in the form of **Field Name=Field Value** (e.g., *Description=This is a test*). Please make sure to set valid values for all required fields in the selected work item type. If you use incorrect field values or field names, the task will fail to create the work item. Like the rest of the task inputs, the field mappings support variables and nested variables.

  We strongly recommend using the special editor for this field by clicking the selection button on the right (three dots). The editor ensures that you set values for all required fields, displays allowed values or special picker controls when necessary, and validates your inputs. Keep in mind, though, that there is no validation of inputs if you use variables instead of fixed values.

  ![Field Mapping Helper](../assets/FieldMappingHelper.png "Mapping values to additional work item fields")

- <a name="associate">**Associate Work Item With Current Build:**</a> Check this option if you want to associate the newly created work item with the current build. This option only takes effect when the task is running as part of a build; it is ignored when running inside a release.

#### Linking
Use the parameters in the *Linking* group to control links from the newly created work item to already existing work items:

![Work Item Linking Parameters](../assets/LinkingInputs.png "Configuring work item links")

- <a name="linkWorkItems">**Link Work Items:**</a> Check this option to enable linking the new work item with one of more existing work items.

- <a name="linkType">**Link Type:**</a> Select the link type that should be used for linking other work items. If the drop-down list is empty, click the refresh button on the right. This option is only visible if linking is enabled.

  **Note:** The link type you select is used for the linked work item, not the created work item. Thus, if you select *Parent*, the linked work item will become the parent of the newly created work item. Please ensure that the combination of *Link Type* and *Link Target* results in a valid link relationship. E.g., if you select *Parent* as the link type and then use a *Target Query* that results in two or more work items, the task will fail to create the work item links since a work item can only have one parent.

- <a name="linkTarget">**Link Target:**</a> Choose *ID* and specify the *Target ID* to link the new work item to a specific work item with a known ID. Select *Query* and enter a valid *Target Query* to link the new work item to one ore more items matching a given work item query. This option is only visible if linking is enabled.

- <a name="targetId">**Target ID:**</a> Enter the ID of the work item that should be linked to the new work item. This option is only visible if linking is enabled and *Link Target* is set to *ID*.

- <a name="targetWiql">**Target Query:**</a> Enter a valid work item query ([WIQL syntax](https://docs.microsoft.com/en-us/vsts/boards/queries/wiql-syntax?view=vsts)) to select the linked work items. You may enter a full query or just the *WHERE* clause of the query as shown in the screenshot. The task will only use the *WHERE* clause and ignore any *SELECT*, *ORDER BY*, or *ASOF* clauses.

#### Duplicates
Use the parameters in the *Duplicates* group to control handling of duplicates that might be created by the task:

![Duplicate Handling Parameters](../assets/DuplicatesInputs.png "Configuring the handling of duplicate work items")

- <a name="preventDuplicates">**Prevent Duplicates:**</a> Check this option to prevent the task from creating duplicate work items. This may be useful if you, e.g.,  want to create a work item for the first failing build but not for subsequent failing builds. Make sure to specify *Key Fields* as well.

- <a name="keyFields">**Key Fields:**</a> List the fields (one per line) that should be used by the task to check whether the new work item would be a duplicate or not. You may use either the field names, field reference names, or a combination of both. To check for duplicates, the task constructs a work item query based on the key fields listed here and the values of those fields in the new work item. If the query returns one or more work items, they are considered duplicates of the new work item and the task does not create a new work item.

  **Example:** You configure the task to create a *Product Backlog Item* with title set to "Some Title" and area path set to "MyProject\Area1", and you configure the key fields *Title*, *Area Path*, and *State*. The task would then create the following query:

  `SELECT System.Id FROM WorkItems WHERE System.Title = 'Some Title' AND System.AreaPath = 'MyProject\Area1' AND System.State = 'New'`

  System.State is checked for the value *New* because that is the initial value that would be used by the new work item.

#### Outputs
Use the parameter in the *Output Variables* group to control the creation of output variables that can be used by subsequent tasks:

![Outputs Parameters](../assets/OutputsInputs.png "Configuring the creation of output variables")

- <a name="createOutputs">**Create Output Variables:**</a> Check this option to enable the creation of output variables. Variables are based on field values of the newly created work item and can be used in subsequent tasks within the same build/release phase.

- <a name="outputVariables">**Output Variables:**</a> To allow maximum flexibility the task does not simply create a predefined set of output variables. Instead you can specify variable-value-mappings (one per line) in the form of **Variable Name=Work Item Value** (e.g., *NewDescription=Description*). This allows you to create just the variables you need for the values you need. You may use either the friendly field name (e.g., *Description*) or the reference name (e.g., *System.Description*) as the work item value. The following values can be used to reference special information of the work item:

  - **CWI.WorkItemUrl** - returns the URL that can be used to view the newly created work item in the browser

  You can reference the output variables like any other variable. If you used the mapping *NewDescription=Description*, you would reference your variable as *$(NewDescription)*.

  **Note:** Make sure to name your output variables carefully when using multiple instances of the *Create Work Item* task in your build or release. If you use the same output variable in multiple tasks, each task overwrites the values from the previous task.

Icons made by [Pavel Kozlov](https://www.flaticon.com/authors/pavel-kozlov) from https://www.flaticon.com is licensed by [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/)