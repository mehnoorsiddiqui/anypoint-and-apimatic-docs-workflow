This repository demonstrates how APIMatic's [portal generation API](https://portal-api-docs.apimatic.io/#/http/generating-api-portal/build-file-reference) can be used to automate the generation of Developer Portals via CI/CD.

## APIMatic Portal Generation API   
  
The Portal Generation API is offered as a standalone solution that is **completely independent** of [APIMatic's web based solution](https://www.apimatic.io/). This is an API first solution and does not come with a GUI.

The Portal Generation API allows the entire Documentation Portal to be managed via a git repository. This includes API Specifications, Markdown Documentation, Cofiguration files as well as static content referenced in the documentation such as images. 

The Portal Generation API expects these files to be provided as a ZIP archive, with the following directory structure:

```
APIMATIC-BUILD.json
spec\
  openapi.json
content\
  guide1.md
  guide2.md
  toc.yml
  custom-section/
    section-overview.md
    toc.yml
static\
  images\
    img1.png
    img2.jpg
  another-static-file.txt
```
In addition to the above directory structure, the following constraints are also applied on the input ZIP archive:
- APIMatic expects a file with name ending with APIMATIC-BUILD.json file in the root directory. For details on the contents of this file, check out our [documentation](https://portal-api-docs.apimatic.io/#/http/generating-api-portal/build-file). 
- The spec folder must contain at least one API specification. This can be in any of the API specification formats supported by APIMatic.
- content and static directories can be skipped if you do not have custom content or static files.

## Automation

This repository contains all the files required to generate a Documentation portal and conforms to the constraints described above.

The contents of this GitHub repository need to be compressed into a ZIP archive and passed to the [Portal Generation](https://portal-api-docs.apimatic.io/#/http/api-endpoints/portal/generate-using-file) Endpoint.
The result is another ZIP archive containing a static Documentation Portal. These files can be deployed to any web server or static hosting platform such as Netlify.

This entire process is automated via the [DeployStaticPortal.yml](https://github.com/apimatic/static-portal-workflow/blob/master/.github/workflows/DeployStaticPortal.yml) workflow.

This workflow can either be triggered manually or
by pushing a change to the master branch of this repository. 
Once triggered, the contents of this repository are zipped and sent to the Portal Generation Endpoint via curl. The response received from this endpoint is unzipped and deployed to Netlify.


## Cool, How do I try it out ?

First off you need to [generate an API KEY](https://portal-api-docs.apimatic.io/#/http/generating-api-portal/auth-keys).

### Try using API Client  
  
  
If you just want to quickly test out the API, zip the contents of this repository and use your favourite API Client to make an API call to the Portal Generation Endpoint :

```
curl -X POST \
  --url 'https://www.apimatic.io/api/portal' \
  -H 'Authorization: X-Auth-Key {x-auth-key}'\
  -F 'file=@your_zip_file.zip'
```

You can deploy the response to any web server to view the sample Documentation Portal.

### Try using GitHub Actions

  
If you want to try out the entire GitHub workflow, you will need to do the following:
1. Fork this repository  
2. Create a new website on Netlify.
3. Create the following Github Repository Secrets:  
    - API_KEY for your APIMatic API Key
    - NETLIFY_AUTH_TOKEN for your Netlify API Key
    - NETLIFY_SITE_ID for the Site ID of the website you created in step 2 
4. Trigger the 'Deploy Static Portal' workflow

Once the workflow completes, your portal should be deployed to Netlify.



