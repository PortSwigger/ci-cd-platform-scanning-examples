# Default Configuration Template

From 2024.5 onwards, you can extract the default configuration template directly from the container using the following command:

`docker run public.ecr.aws/portswigger/enterprise-scan-container:2024.5 --config-template`

If you would like to create a local copy of the template as a starting point for your own customisations, you can use the following command:

`docker run public.ecr.aws/portswigger/enterprise-scan-container:2024.5 --config-template > burp_config.yml`

Remember to substitute the container label with the version you are actually using 😉

## Example output from version 2024.5

```yaml
# Use this file to configure your scan. Enter the values after the colon for each parameter.
# Comments are marked by the # symbol. List entries are marked by two spaces, a hyphen and then another space.
# Values of the format ${BURP_ENVIRONMENT_VARIABLE} are read from environment variables but can be replaced with hard-coded values here if required.
# An environment variable may be optional with a default. For example, ${BURP_REPORT_FILE_PATH:-burp_junit_report.xml}. In this example if the environment variable is not defined or blank, the value "burp_junit_report.xml" is used.
#
# Some environment variable names allow lists and complex structures.
# Environment variables with names other than those listed below are interpreted as single strings.
# Values for the following environment variables are interpreted as comma-separated lists:
#
# BURP_START_URLS
# BURP_SITE_IN_SCOPE_URL_PREFIXES
# BURP_SITE_OUT_OF_SCOPE_URL_PREFIXES
# BURP_RECORDED_LOGINS
# BURP_SCAN_CONFIGURATIONS
# BURP_CUSTOM_SCAN_CONFIGURATION_FILES
#
# For example, the environment variable BURP_START_URLS with multiple values:
#
# $ BURP_START_URLS="www.vulnerable-website.com,https://www.ginandjuice.shop"
#
# It can be used as follows:
#
# site:
#   scope:
#     startUrls: ${BURP_START_URLS}
#
# It is equivalent to:
#
# site:
#   scope:
#     startUrls:
#     - www.vulnerable-website.com
#     - https://www.ginandjuice.shop
#
# If you use an environment variable to provide a list, don't include the list in the file. The scan will fail.
#
# Environment variables for more complex structures are in JSON format:
#
# BURP_LOGIN_CREDENTIALS
# BURP_REPORTING_IGNORED_ISSUES
# BURP_HEADERS_COOKIES_CONFIG_HEADERS
# BURP_HEADERS_COOKIES_CONFIG_COOKIES
# BURP_PLATFORM_AUTHENTICATION
# BURP_PROXIES
#
# This is an  example of the JSON for the environment variable BURP_LOGIN_CREDENTIALS:
#
# $ BURP_LOGIN_CREDENTIALS='[{"username": "user1", "password": "password1"}, {"username": "user2", "password": "password2"}]'
#
# It can be used as follows:
#
# logins:
#   loginCredentials: ${BURP_LOGIN_CREDENTIALS}
#
# It is equivalent to:
#
# logins:
#   loginCredentials:
#   - username: user1
#     password: password1
#   - username: user2
#     password: password2
#
# The JSON for BURP_REPORTING_IGNORED_ISSUES is more complex:
#
# $ BURP_REPORTING_IGNORED_ISSUES='[{"name": "OS command injection", "paths": ["www.vulnerable-website.com/path1", ".*/api/v1"]}, {"name": "SQL injection", "paths": ["www.vulnerable-website.com/path2", ".*/api/[0-9]*"]}]'
#
# It can be used as follows:
#
# reporting:
#   ignoredIssues: ${BURP_REPORTING_IGNORED_ISSUES}
#
# It is equivalent to:
# reporting:
#   ignoredIssues:
#   - name: OS command injection
#     paths:
#     - www.vulnerable-website.com/path1
#     - .*/api/v1
#   - name: SQL injection
#     paths:
#     - www.vulnerable-website.com/path2
#     - .*/api/[0-9]*

enterpriseServer:
  # Enter the URL for your Enterprise server.
  url: ${BURP_ENTERPRISE_SERVER_URL}

  # Enter the API key for your API user.
  apiKey: ${BURP_ENTERPRISE_API_KEY}

  # If you are using a self-signed TLS certificate on your Enterprise server, add the public key of the certificate here. For example:
  # tlsCertificate: |-
  #   -----BEGIN CERTIFICATE-----
  #   ...
  #   -----END CERTIFICATE-----
  tlsCertificate: ${BURP_ENTERPRISE_SERVER_TLS_CERTIFICATE}

  # If you need to use a proxy to connect to your Enterprise server, add the url and, optionally, the authentication credentials.
  proxyUrl: ${BURP_ENTERPRISE_SERVER_PROXY_URL}
  proxyUsername: ${BURP_ENTERPRISE_SERVER_PROXY_USERNAME}
  proxyPassword: ${BURP_ENTERPRISE_SERVER_PROXY_PASSWORD}

site:
  # If you define a correlationId, you can view the results of your scan on your Burp Suite Enterprise Edition dashboard.
  # Burp Suite Enterprise Edition uses the correlation ID as the site name. You can use a maximum of 64 characters.
  correlationId: ${BURP_CORRELATION_ID}

  scope:
    # You need to provide a list of one or more URLs that Burp Scanner starts scanning from.  For example:
    #  startUrls:
    #  - www.vulnerable-website.com/
    #  - https://www.ginandjuice.shop
    #
    # For a single URL, you can use BURP_START_URL. For example:
    #
    #     $ BURP_START_URL="www.vulnerable-website.com/"
    #
    #     startUrls:
    #     - ${BURP_START_URL}
    #
    # For one or more URL, use BURP_START_URLS. For example:
    #
    #     $ BURP_START_URLS="www.vulnerable-website.com/,https://www.ginandjuice.shop"
    #
    #     startUrls: ${BURP_START_URLS}
    startUrls:
      - ${BURP_START_URL}

    # Detailed scope configuration settings
    # A list of URLs that you want to allow Burp Scanner to visit. The startUrls are included automatically. For example:
    # inScopeUrlPrefixes:
    # - include1.ginandjuice.shop
    # - include2.ginandjuice.shop
    #
    # This can be an environment variable BURP_SITE_IN_SCOPE_URL_PREFIXES which can contain zero or more in-scope URLs. Usage:
    #
    # $ BURP_SITE_IN_SCOPE_URL_PREFIXES="include1.ginandjuice.shop,include2.ginandjuice.shop"
    #
    # inScopeUrlPrefixes: ${BURP_SITE_IN_SCOPE_URL_PREFIXES}
    inScopeUrlPrefixes: ${BURP_SITE_IN_SCOPE_URL_PREFIXES}

    # A list of URLs that you don't allow Burp Scanner to visit. Use this list to avoid scanning sites you don't have permission to scan. Example:
    # outOfScopeUrlPrefixes:
    # - ignore1.ginandjuice.shop
    # - ignore2.ginandjuice.shop
    #
    # This can be an environment variable BURP_SITE_OUT_OF_SCOPE_URL_PREFIXES that contains zero or more out-of-scope URLs. For example:
    #
    # $ BURP_SITE_OUT_OF_SCOPE_URL_PREFIXES="ignore1.ginandjuice.shop,ignore2.ginandjuice.shop"
    #
    # outOfScopeUrlPrefixes: ${BURP_SITE_OUT_OF_SCOPE_URL_PREFIXES}
    outOfScopeUrlPrefixes: ${BURP_SITE_OUT_OF_SCOPE_URL_PREFIXES}

scanConfigurations:
  # Select from Burp Scanner's built-in scan configurations.
  # Enter the name of the scan configuration as a list. The names of the scan configurations are case-sensitive.
  # For a list of the available configurations, see https://portswigger.net/burp/documentation/scanner/scan-configurations/burp-scanner-built-in-configs
  # You can stack scan configurations. If settings in two or more scan configurations conflict, the setting from the last configuration in the list takes precedence.
  # builtin:
  # - "Crawl and Audit - CICD Optimized"
  # - "Crawl limit - 10 minutes"
  #
  # You can replace the list with the environment variable BURP_SCAN_CONFIGURATIONS. This can contain zero or more built-in scan configuration names. For example:
  #
  # $ BURP_SCAN_CONFIGURATIONS="Crawl and Audit - CICD Optimized,Crawl limit - 10 minutes"
  #
  # builtIn: ${BURP_SCAN_CONFIGURATIONS}
  builtIn: ${BURP_SCAN_CONFIGURATIONS:-Crawl and Audit - CICD Optimized}

  # Use a custom scan configuration. You can export custom scan configurations from Burp Suite Enterprise Edition in JSON format. Enter the paths to these files as a list.
  # custom:
  # - custom-config1.json
  # - custom-config2.json
  #
  # You can replace this list with the environment variable BURP_CUSTOM_SCAN_CONFIGURATION_FILES. This can contain zero or more custom scan configuration file paths. For example:
  #
  # $ BURP_CUSTOM_SCAN_CONFIGURATION_FILES="custom-config1.json,custom-config2.json"
  #
  # custom: ${BURP_CUSTOM_SCAN_CONFIGURATION_FILES}
  custom: ${BURP_CUSTOM_SCAN_CONFIGURATION_FILES}

logins:
  # Enter a list of username and password pairs. For example:
  #  loginCredentials:
  #  - username: user1
  #    password: password1
  #  - username: user2
  #    password: password2
  #
  # You can use the environment variable BURP_RECORDED_LOGINS.  This can contain a JSON array containing zero or more login credentials. For example:
  #
  # $ BURP_LOGIN_CREDENTIALS='[{"username": "user1", "password": "password1"}, {"username": "user2", "password": "password2"}]'
  #
  # loginCredentials: ${BURP_LOGIN_CREDENTIALS}
  loginCredentials: ${BURP_LOGIN_CREDENTIALS}

  # A list of paths to files that contain the recorded login JSON scripts.
  # To record a login sequence, follow the instructions for recording a login sequence, and save the script as a JSON file:
  # https://portswigger.net/burp/documentation/scanner/authenticated-scanning/using-recorded-logins.html/#recording-a-login-sequence
  # recordedLogins:
  # - user-login-sequence.json
  # - admin-login-sequence.json
  # - admin-login-sequence.json
  #
  # You can use the environment variable BURP_RECORDED_LOGINS.  This can contain zero or more login credentials. For example:
  #
  # $ BURP_RECORDED_LOGINS="user-login-sequence.json,admin-login-sequence.json"
  #
  # recordedLogins: ${BURP_RECORDED_LOGINS}
  recordedLogins: ${BURP_RECORDED_LOGINS}

reporting:
  # You can ignore specific vulnerabilities on specific paths, so that they do not cause the build to fail.
  # Burp Scanner still looks for these vulnerabilities and records them in the results.
  # For a list of vulnerabilities, see https://portswigger.net/burp/documentation/scanner/vulnerabilities-list.
  # You can use regex for the paths. If you omit the path, the issue is ignored everywhere.
  # For example:
  # - name: OS command injection
  #   paths:
  #   - www.vulnerable-website.com/path1
  #   - .*/api/v1
  # - name: SQL injection
  #   paths:
  #   - www.vulnerable-website.com/path2
  #   - .*/api/[0-9]*
  #
  # You can use the environment variable BURP_REPORTING_IGNORED_ISSUES. This can contain a JSON array containing zero or more ignored issues. For example:
  #
  # $ BURP_REPORTING_IGNORED_ISSUES='[{"name": "OS command injection", "paths": ["www.vulnerable-website.com/path1", ".*/api/v1"]},{"name": "SQL injection", "paths": ["www.vulnerable-website.com/path2",".*/api/[0-9]*"]}]'
  #
  # ignoredIssues: ${BURP_REPORTING_IGNORED_ISSUES}
  ignoredIssues: ${BURP_REPORTING_IGNORED_ISSUES}

  # You need to define the destination path for the report.
  reportFilePath: ${BURP_REPORT_FILE_PATH:-burp_junit_report.xml}

  # You can specify the format of the reports produced. Select JUNIT or BURP_XML.
  # Burp XML report are written to the working directory as burp_xml_report.xml
  # Defaults to JUnit only.
  reportFormats:
    - ${BURP_REPORT_FORMAT:-JUNIT}

  # Enter a minimum severity and a minimum confidence.
  # If Burp Scanner detects an issue with at least this severity and confidence, it finishes with a non-zero exit code.
  # This tells your CI/CD system to fail the pipeline step.
  threshold:
    # Can be set to INFO, LOW, MEDIUM, HIGH.
    minimumSeverity: ${BURP_REPORT_THRESHOLD_MINIMUM_SEVERITY:-LOW}

    # Can be set to TENTATIVE, FIRM, CERTAIN.
    minimumConfidence: ${BURP_REPORT_THRESHOLD_MINIMUM_CONFIDENCE:-TENTATIVE}

    # Can be either true or false:
    # enabled: true  - will make the container to exit with a non-zero exit code
    # enabled: false - will make the container exit with return code 0 (success) regardless of any issues the scan finds.
    # This is useful if you want CI build steps to pass even if the scanner finds vulnerabilities.
    enabled: ${BURP_REPORT_THRESHOLD_ENABLED:-true}

headersCookiesConfig:
  # A list of additional request headers, including name, value and scope prefix
  # headers:
  #  - name: name1
  #    scopePrefix: scopePrefix1
  #    value: value1
  #  - name: name12
  #    scopePrefix: scopePrefix2
  #    value: value2
  #
  # You can use the environment variable BURP_HEADERS_COOKIES_CONFIG_HEADERS.  This can contain a JSON array containing zero or more HTTP headers. For example:
  #
  # $ BURP_HEADERS_COOKIES_CONFIG_HEADERS='[{"name": "name1","scopePrefix": "scopePrefix1","value": "value1"}, {"name": "name2", "scopePrefix": "scopePrefix2", "value": "value2"}]'
  #
  # headers: ${BURP_HEADERS_COOKIES_CONFIG_HEADERS}
  headers: ${BURP_HEADERS_COOKIES_CONFIG_HEADERS}
  # A list of additional request cookies, including name, value and scope prefix
  # cookies:
  #  - name: name1
  #    scopePrefix: scopePrefix1
  #    value: value1
  #  - name: name12
  #    scopePrefix: scopePrefix2
  #    value: value2
  #
  # You can use the environment variable BURP_HEADERS_COOKIES_CONFIG_COOKIES. This can contain a JSON array containing zero or more HTTP cookies. For example:
  #
  # $ BURP_HEADERS_COOKIES_CONFIG_COOKIES='[{"name": "name1","scopePrefix": "scopePrefix1","value": "value1"}, {"name": "name2", "scopePrefix": "scopePrefix2", "value": "value2"}]'
  #
  # cookies: ${BURP_HEADERS_COOKIES_CONFIG_COOKIES}
  cookies: ${BURP_HEADERS_COOKIES_CONFIG_COOKIES}

  # You can specify a list of additional platform authentication details
  # platformAuthentication:
  # - type: Choose from BASIC, NTLM_V1, or NTLM_V2.
  #   destinationHost: localhost
  #   username: username
  #   password: password
  #   domain: domain - Only required for NTLM authentication
  #   domainHostname: hostname - Only required for NTLM authentication
  #
  # You can use the environment variable BURP_PLATFORM_AUTHENTICATION. This can contain a JSON array containing zero or more platform authentication configurations. For example:
  #
  # $ BURP_PLATFORM_AUTHENTICATION='[{"type": "basic", "destinationHost": "localhost", "username": "username", "password": "password"}, {"type": "ntlm_v1", "destinationHost": "localhost", "username": "username", "password": "password", "domain": "domain", "domainHostname": "hostname"}]'
  #
  # platformAuthentication: ${BURP_PLATFORM_AUTHENTICATION}
platformAuthentication: ${BURP_PLATFORM_AUTHENTICATION}

  # A list of proxy server configurations
  # proxies:
  # - authentication:
  #     authType: Choose from NONE, BASIC, NTLM_V1, or NTLM_V2.
  #     username: username
  #     password: password
  #     domain: domain
  #     domainHostname: hostname
  #   destinationHost: localHost
  #   proxyHost: host
  #   proxyPort: 8080
  #
  # You can use the environment variable BURP_PROXIES. This can contain a JSON array containing zero or more proxy configurations. For example:
  #
  # $ BURP_PROXIES='[{"authentication": {"authType": "NONE", "username": "username", "password": "password", "domain": "domain", "domainHostname": "domainHostname"}, "destinationHost": "localhost", "proxyHost": "host", "proxyPort": "8080"}]'
  #
  # proxies: ${BURP_PROXIES}
proxies: ${BURP_PROXIES}
```

> [!NOTE]
> The example templates in this repo are now deprecated in favour of extracting the current template directly from the container, and they will be removed in due course.
