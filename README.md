[![Build Status](https://travis-ci.org/devarsh1997/taxonworks_api.svg?branch=master)](https://travis-ci.org/devarsh1997/taxonworks_api)

# taxonworks_api

[http://api.taxonworks.org](http://api.taxonworks.org)

Documentation for the TaxonWorks workbench API.  

# Status

TLDR - We have lots of non externally documented, non `/api/v1` nodes that could be aliased for experimenting. Once we do a major branch merge in the near future we will turn our focus to getting those endpoints cleaned up, and exposed in a proper fashion here.  

See the issues here as to our present thinking/considerations and next steps.

Issues pertaining to implementation (actual code) also exist as [TaxonWorks issues](https://github.com/SpeciesFileGroup/taxonworks/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3AAPI)

## Details

The `/api/v1` endpoints exist as an early proof of concept, they included our tests for tokenized user access to the API.  They are not particularly useful.

We ran a much more intensive experiment, the matrix_row_coder, you can see how we mocked API calls here: https://github.com/SpeciesFileGroup/matrix_row_coder/tree/master/src/request/mockRequests.

Recently, all of the VUE.js based views are JSON endpoint based. Unfortunately these are not yet documented beyond their use in the code.  If you experiment with the `Edit taxon name` task, watching the server log, you'll get a feel for the requests there. 

There exist JSON responses for many of the basic CRUD requests inside the application, just append '.json' to the request.

# Experimenting with endpoints not in `/api/v1/`

You can add existing internal endpoints to `api/v1` for the purposes of experimenting by aliasing them in the `config/routes.rb` file relatively easily.  To get a resources CRUD just add that resource to API scope as you would see it above in the application endpoint

```Ruby

  scope :api, :defaults => { :format => :json }, :constraints => { id: /\d+/ } do
    scope  '/v1' do
    
    # Add TaxonNames endpoints
    resources taxon_names
    
    # ...
    
    end
  end
```

# Getting a list of all the endpoints

From your endpoint you can do `rake routes` to get a list of all endpoints.

# Building 

Documentation is built with [gulp](https://gulpjs.com/). To regenerate documentation just `gulp`.

# Running api-console

Allows to explore documentation but also try out the API right from the documentation page against a local rails server (localhost:3000).

Use `npm install` if you haven't already. Then just run `npm start -- --open` (`-- --open` is optional, it will make your browser open the URL the console is being served from).

Once it is ready to explore you should see something like this:
```bash
$ npm start -- --open

(OUTPUT OMITTED)

API console build ready.
Thanks for using our API tools!

Files in this directory are available under the following URLs
        applications: http://127.0.0.1:8081
```
Remember to have a taxonworks instance running at localhost:3000 (`rails s` where you have taxonworks repo cloned) before sending API requests though the console.

**NOTE**: It might display a white screen when the browser opens the URL (specially true with Firefox, Chrome seems inmune to this problem). Refresh a few times until it shows the documentation.
