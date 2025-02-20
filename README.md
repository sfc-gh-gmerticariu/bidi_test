# Example BiDi App

This app contains an example PrPr / vendored custom component.

Run the following in a cloud workspace to create this app:
```sql
-- Default role.
USE ROLE SYSADMIN;
CREATE DATABASE STREAMLIT;
CREATE SCHEMA APPS;

-- Must be account admin to create integration.
USE ROLE ACCOUNTADMIN;
CREATE OR REPLACE API INTEGRATION github
    API_PROVIDER = git_https_api
    API_ALLOWED_PREFIXES = ('https://github.com')
    ENABLED = true
    ALLOWED_AUTHENTICATION_SECRETS = all;

GRANT USAGE ON INTEGRATION github TO ROLE SYSADMIN;

USE ROLE SYSADMIN;
CREATE OR REPLACE GIT REPOSITORY streamlit.apps.example_bidi
	ORIGIN = 'https://github.com/sfc-gh-wschmitt/bidi_test'
	API_INTEGRATION = 'GITHUB';

CREATE OR REPLACE STREAMLIT streamlit.apps.test
FROM @streamlit.apps.example_bidi/branches/main
    MAIN_FILE='streamlit_app.py'
    QUERY_WAREHOUSE='regress';
```
