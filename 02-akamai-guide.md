# Akamai configurations to all NPEs

Akamai is a global Content Delivery Network (CDN), cybersecurity and cloud service provider. It assists in delivering and distributing content, such as web pages, video content and web applications, to users across the world in a fast and secure manner. This involves setting up and customizing Akamai services for all non-production environments (NPEs). Non-production environments, separate from the live production environment, are used for testing, development and staging.

The steps below are performed to apply the Akamai configuration to all non-production environments, except npe002 and npe03 environments.

- DNS request: A DNS request is generated in ServiceNow for all Non-Production Environments (NPEs) at the same time. This request aims to create a CNAME record for each environment, which points to the canonical name `web-dev2.shell.com.edgekey.net`.

- Akamai certificate: Once the DNS entries are created, we incorporate these DNS names into the Akamai certificate. Akamai is a Content Delivery Network (CDN) and cloud service provider. Adding the DNS names to the certificate ensures secure, encrypted connections to these domains. This process is carried out for all NPEs simultaneously.

- Clone property: The configurations of `npe20` are duplicated across all other NPEs (excluding `npe002` and `npe03`). This is achieved using an interface provided by Akamai.

- Change origin hostname: The origin hostname is modified by cloning the properties. The origin hostname refers to the address of the server where the original, definitive version of the website is hosted.

  - When specifying the origin server's hostname, it's necessary to define the property variables `PMUSER_HOSTNAME_PART1` and set the variable in the `Behaviour` section.

  - An expression is constructed to set up the support variable, represented as `({{builtin.AK_HOST}})`. Additionally, a regular expression, specifically `^(.*).shell.co.uk$`, is used to define the hostnames.

  - The hostname for the origin server is set as `shell-{{user.PMUSER_HOSTNAME_PART1}}.adobecqms.net`.

- Update dispatcher configuration/GitHub workflows:

  - Essential code modifications are carried out in both the dispatcher configuration and GitHub workflows. This process entails updating settings, changing environment variables, or adjusting scripts. The phrase 'rewrite setup' pertains to URL rewrite rules, which govern how Akamai rewrites URLs.

  - Velocity templates for each of Akamai's non-production environments (excluding npe002 and npe03) are utilized in Maven project configuration (pom.xml). For additional information, please refer to the following URL: https://github.com/sede-x/oneshell-complete/blob/develop/shell-dispatcher-base/pom.xml.

  - Additionally, a separate `sorted_map.vm` file is created as part of the URL rewriting configuration. Velocity templates are employed to generate text files based on a set of input values. We ensure that the templates accurately use the input values to produce the desired output. This process includes verifying that variables are substituted correctly, that conditional logic operates as expected, and that the overall structure of the output file is accurate.

  - The `sorted_map.vm` file is utilized to establish a set of URL rewrite rules. These rules serve to redirect one URL to another, add or remove query parameters, or execute other transformations on URLs. A check should be implemented to ensure that each rule is accurately defined and corresponds to the URLs it is expected to match.

- Updated Amidala runtime test workflow:

  - The Amidala runtime test workflow is revised to ensure it connects to the correct endpoint.
  - The publish URLs for most of the non-production environments are updated. Previously, publish URLs pointed to `shell-npexx.adobecqms.net`. Now, publish URLs have been changed to `npexx.shell.co.uk`. This change is applied to all non-production environments, with the exception of `npe002` and `npe03`. These continue to use the old publish URL `shell-npexx.adobecqms.net`.

  - For more information, please refer to the following URL: https://github.com/sede-x/oneshell-complete/blob/feature/653991-documentation-CI/CD-tool-devops/.github/workflows/amidala-runtime-tests.yml.

- The Akamai Purge Agent is installed on the author instance of all the NPEs. This action automatically clears the cache on our servers.

## Akamai certificate pinning

- Akamai certificate pinning involves designating specific certificates to be trusted. When these pinned certificates near their expiration date, an alert is generated to prompt their renewal.

- Access to Akamai control center:

  - Visit https://control.akamai.com/ and log in. Make sure that the login account is named 'Shell Information Technology International Limited'.

- Navigate to the properties section:

  - From the control center, navigate to the CDN and then to the properties section.

- Select the appropriate environment:

  - Choose the environment where the certificate needs to be pinned. This could be production, pre-production, development, etc.

- Create a new version:

  - Select the most recent version and create a new version based on it.

- Add a new certificate:

  - In the 'Specific Certificates (pinning)' section, click on 'Add Certificate'.

- Retrieve the certificate from the origin:

  - Choose the 'Retrieve from Origin' option. Then, paste the hostname where the certificate had already been renewed.

- Save the certificate:

  - Click on 'Add Certificate'. After the certificate has been added, save the changes to the version.

### Groovy script execution

- Access the AEM instance:

  - Log in to the Adobe Experience Manager (AEM) instance.

- Enable the AEM Groovy Console bundle:

  - Go to the System console and enable the AEM Groovy console bundle. This bundle enables the execution of Groovy scripts in AEM.

- Execute the AEM Easy Content Upgrade:

  - Go to AEM Tools and execute the AEM Easy Content Upgrade. This tool aids in automating content upgrades in AEM.

- Search and execute the scripts:

  - Search for the scripts provided in the release details. Initially, execute the dry run script, which simulates the changes without actually implementing them. This allows you to verify what changes would be made. After reviewing the dry run, execute the wet run script to apply the changes.

### Akamai cache flushing

Akamai cache flushing, also referred to as 'Cache Purging', is the process of deleting cached content from Akamai's edge servers. Here are the general steps to carry out this process:

- Log in to Akamai Control Center:
  - Visit the Akamai Control Center at https://control.akamai.com/ and log in.
- Navigate to the Purge section:
  - After logging in, navigate to the 'Purge' section, typically located under the 'Publish' menu.
- Select the type of purge:
  - Decide whether to purge by URL(s) or CP code(s).
  - URL(s): This option lets you specify the exact URLs that need purging.
  - CP Code(s): This option lets you purge all content associated with specific CP codes.
- Choose the network:
  - Decide whether a purge is needed from the staging network, the production network, or both.
- Submit the purge request:
  - Click on the 'Purge' or 'Submit' button to initiate the purge.
