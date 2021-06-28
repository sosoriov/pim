# Nuxeo - IMDC INSTALLATION VERSION 1.0

This version includes the import of the existing 
  - projects
  - project references
  - imdc employees
  - clients
  - contacts
  - CE
  - Fiches
  - Curriculum

### Version
1.0.0
### Deployment - IMDC server
As this is the first deploy of PIM, we have to deploy it in two steps:

#### Step 1
Create  a Release Version in Nuxeo Studio and be sure the following configuration is active:
> You have to disable some Event Handlers
> and the the user configuration before import the data
  - Event Handlers: check the following EH contains a dummy restriction in currentDocument State to avoid its execution
    *   *eh_create_project*
    *   *eh_set_default_project_reference*
    *   *eh_set_project_reference_available*
    *   *eh_edit_curriculum_new*
- Users and groups: Be sure you have the creation policy is set on Don't override existing values
- Vocabularies: Be sure all the creation policy are set on Don't override existing values

Install the release package in the server and restart the server

**Note:** First you have to create all the groups available in Studio and the other ones to approve the offer signature. Additionaly you have to create an user to use the Offer Excel Template. The user credentials has to be:
username:offer_manager_user
pass:123

##### Importing data

Import the information in the following order. The data is available on **K:\PROJECTS\00\00083\00083-03 Ontwikkelingen intern\PMP\nuxeo_import_data\FINAL DATA TO IMPORT**
* [IMDC users] - employees_to_import7.csv
* [Companies] - companies.csv
* [clients] - contacts.csv
* [Projects] - projects_full_version_to_import.csv
    * Run additional script to syncronized project clients and client_id Nuxeo repository: **nuxeo_update_project_clients.py** It uses the Nuxeo REST API
    * Check the **end_date** field in the projects CSV file. If the data is wrong you have to run the following script to fix it directly in Nuxeo: **projects_update_end_date**
* [Project References] - project_references_full_version_to_import.csv
* [IMDC CVs] - The information will be imported using the REST API run the script: **get_new_cv_data.py**
    * Be sure the URL_SERVER is on http://imdc-nuxeo:8080/nuxeo/
    * Loop over all IMDC user in the main function
* [CE] - copy all the files to the Nuxeo data directory, then import as usual using the CSV file that must be located in the same folder
* [FICHES] - copy all the files to the Nuxeo data directory, then import as usual using the CSV file that must be located in the same folder

#### Step 2
Create a new relase in Studio and be sure the following EH are enable:
  - Event Handlers:
    *   *eh_create_project*
    *   *eh_set_project_reference_available*

Install the new version on *IMDC server*.

### Template installations
#### Offer Word Template
1. Create a nuxeo Template under the Templates root:
    * Select offer Word type
    * Automatically associated to... None
    * Rendition type: Delivery
    * Processor: XDocReportProcessor
    * Check this template is editable on Office
2. Create a Word Offer document under Templates root folder
3. Assign the template created in Step 1 to the document created in Step 2
4. Repeat the steps 1 to 3 for the different templates: NL, FR, SP

#### Curriculum Word Template
1. Create a nuxeo Template under the Templates root:
    * Select Curriculum offer
    * Automatically associated to: Curriculum Offer
    * Rendition type: None
    * Processor: XDocReportProcessor

#### Offer Excel Template
1. Create a nuxeo Template under the Templates root: *Remember the user has to render and upload the Excel file to the Excel Offer Document*
    * Select Excel offer document
    * Automatically associated to: Excel offer document
    * Rendition type: None
    * Processor: JXLSProcessor

### Additional Notes

##### Server Maintenence. Install uninstall local packages via command window:
https://jira.nuxeo.com/browse/SUPNXP-15163?filter=-3

#### Restart OpenOffice service
```sh
ps aux | grep soffice
```
**You should get an output similar to this:**
nuxeo    21239  0.1  0.4 763584 38152 ?        Sl   14:39   0:00 /usr/lib64/libreoffice/program/soffice.bin --accept=socket,host=127.0.0.1,port=2003;urp; -env:UserInstallation=file:///var/lib/nuxeo/server/tmp/.jodconverter_socket_host-127.0.0.1_port-2003_71 env:BUNDLED_EXTENSIONS=file:///var/lib/nuxeo/server/tmp/.jodconverter_bundlesdir_71 --headless --nocrashreport --nodefault --nofirststartwizard --nolockcheck --nologo --norestore
root     21262  0.0  0.0 112640   948 pts/0    S+   14:41   0:00 grep --color=auto soffice

* In the second column you can find the process ID (PID). In this case 21239.
* Now kill the process using the command kill <PID>. In this case kill 21239
* Nuxeo will now restart the service.


graph TD
A[Christmas] -->|Get money| B(Go shopping)
B --> C{Let me think}
C -->|One| D[Laptop]
C -->|Two| E[iPhone]
C -->|Three| F[fa:fa-car Car]

