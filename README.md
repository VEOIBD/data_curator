# Data Curator App

## Introduction

The _Data Curator App_ is an R Shiny app that serves as the _frontend_ to the [schematic Python package](github.com/sage-Bionetworks/schematic/). It allows data contributors to easily annotate, validate and submit their metadata.


## Quickstart {#quickstart}

Sage Bionetworks hosts a version of Data Curator App for its collaborators. [Access it here](link TBD).  
To configure your project for this version, edit [dcc_config.csv](dcc_config.csv) and submit a pull request.
[dcc_config.csv](dcc_config.csv) contains the following. **Bold fields** are required:

**project_name**: The display name of your project  
**synapse_asset_view**: The synapse ID of your project's asset view  
**data_model_url**: A URL to your data model. Must be the **raw** file if using GitHub  
**template_menu_config_file**: www/template_config/<your-project>_config.json  
**manifest_output_format**: "excel"  
**submit_use_schema_labels**: (default TRUE) TRUE or FALSE  
**submit_table_manipulation**: (default "replace") "replace" or "upsert"  
**use_compliance_dashboard**: (default FALSE) TRUE or FALSE  
primary_col: (default Sage theme) hexadecimal color code  
secondary_col; (default Sage theme) hexadecimal color code  
sidebar_col: (default Sage theme) hexadecimal color code  
        
You will need:  
- A Synapse asset view for you project  
- A [data model](#datamodel)  

For more customization, you can also provide:  
- A json file for manifest templates  
- Schematic options  
- Display the compliance dashboard  
- A custom logo and color scheme  

## Setup a local instance of DCA

### 1.  Clone this repo.

        git clone https://github.com/Sage-Bionetworks/data_curator.git
        cd data_curator
        
### 2. Install required R packages

        R -e "renv::restore()"
      
### 3. Set up [schematic](github.com/sage-Bionetworks/schematic/)

DCA can use Schematic through [reticulate](https://rstudio.github.io/reticulate/) or a REST API.

Using Schematic with reticulate requires python 3.9 or greater. Create a python virtual environment named `.venv` and install schematicpy through [pypi](https://pypi.org/project/schematicpy/) or from [GitHub](github.com/sage-Bionetworks/schematic/)  using [poetry](https://python-poetry.org/docs/). Follow the links to Schematic for more details on installation.

        # python virtual env must be named .venv
        `python3 -m venv .venv`
        
        # For pypi release of schematic, run this line
        `pip3 install schematicpy` 
        
        # Or for development schematic, run the following. Note you'll need to install poetry.
        `git clone https://github.com/Sage-Bionetworks/schematic.git`
        `cd schematic`
        `poetry shell`
        `poetry install`
        
        # At this point you can also run the REST API service locally
        # This will be accessible at http://0.0.0.0:3001
        `poetry run python3 run_api.py`
        
To use Schematic through its REST API, run the service locally using the commands above. Or access [Schematic hosted by Sage Bionetwork](link TBD).

### 4. Configure App {#configureapp}

Many app and schematic configurations are set in `dcc_config.yml` as described in [Quickstart](#quickstart). The following are stored as environment variables. Add these to `.Renviron`.

**Schematic configurations**  
**DCA_SCHEMATIC_API_TYPE**: "rest", "reticulate", or "offline"  
**DCA_API_HOST**: "" (blank string) if not using the REST API, otherwise URL to schematic service  
**DCA_API_PORT**: "" (blank string) if not using the REST API **LOCALLY**, otherwise the port. Usually 3001.  
        
**OAuth-related variables**  
**DCA_CLIENT_ID**: OAuth client ID  
**DCA_CLIENT_SECRET**: OAuth client secret  
**DCA_APP_URL**: OAuth redirect URL  
        
### Data Model Configuration {#datamodel}

The app configuration file `www/config.json` will be used to adapt the schema dropdown menu in the app. The `config.json` file will be automatically created in the deployment workflow.

For local testing, run below snippet to generate `www/config.json` and check the [docs](docs/app_configuration.md#schema-configuration) how to modify it:

1.  Create a repo for your data model using this [template](https://github.com/Sage-Bionetworks/data-models)

2.  Clone your data model repo, i.e:

        git clone https://github.com/Sage-Bionetworks/data-models

3.  Create `config.json` and placed it in the `www` folder

        python3 .github/generate_config_json.py \
          -jd data-models/example.model.jsonld \
          -schema 'Sage-Bionetworks/data-models' \
          -service Sage-Bionetworks/schematic'


## Authentication

This utilizes a Synapse Authentication (OAuth) client (code motivated by [ShinyOAuthExample](https://github.com/brucehoff/ShinyOAuthExample) and [app.R](https://gist.github.com/jcheng5/44bd750764713b5a1df7d9daf5538aea). Each application is required to have its own OAuth client as these clients cannot be shared between one another. View instructions [here](https://docs.synapse.org/articles/using_synapse_as_an_oauth_server.html) to learn how to request a client. Once you obtain the client, make sure to add the corresponding [environment variables](#configureapp)


## Deployment

To deploy the app to shinyapps.io, please follow the instructions in the [shinyapps_deploy.md](./shinyapps_deploy.md).

## Contributors

Main contributors and developers:

- [Rongrong Chai](https://github.com/rrchai)
- [Anthony Williams](https://github.com/afwillia)
- [Milen Nikolov](https://github.com/milen-sage)
- [Loren Wolfe](https://github.com/lakikowolfe)
- [Robert Allaway](https://github.com/allaway)
- [Bruno Grande](https://github.com/BrunoGrandePhD)
- [Xengie Doan](https://github.com/xdoan)
- [Sujay Patil](https://github.com/sujaypatil96)

<!-- Links -->

[schematic]: https://github.com/Sage-Bionetworks/schematic/tree/develop
[poetry]: https://github.com/python-poetry/poetry
