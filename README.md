# Default Configuration Template

From 2024.5 onwards, you can extract the default configuration template directly from the container using the following command:

`docker run public.ecr.aws/portswigger/enterprise-scan-container:2024.5 --config-template`

If you would like to create a local copy of the template as a starting point for your own customisations, you can use the following command:

`docker run public.ecr.aws/portswigger/enterprise-scan-container:2024.5 --config-template > burp_config.yml`

Remember to substitute the conatiner label with the version you are actually using 😉

> [!NOTE]
> The example templates in this repo are now deprecated in favour of extracting the current template directly from the container, and they will be removed in due course.
