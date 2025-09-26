# IPQualityScore OpenAPI Definition

This OpenAPI Definition is provided for the benefit of IPQualityScore customers
to assist you in using our API. Please refer to our API Documentation. Where the
documentation differs from this definition, the documentation should override
this definition.

Documentation is available at: https://www.ipqualityscore.com/documentation/overview

---

Copyright IPQualityScore LLC 2023

This definition is provided in a multi-file directory hierarchy. `openapi.yaml`
in the root directory is the root of the definition.

OpenAPI cannot, nor does it intend to, define all possible features and aspects
of an API. Therefore, there may be features and aspects of the IPQS Fraud
Prevention APIs that are not represented here.

Contact IPQualityScore support for further questions at
https://ipqualityscore.com

## Importing Into Postman
You can import this OpenAPI definition into Postman, and it will automatically
generate a Postman collection which you can use to test and explore our API.

There are two ways to import the definition into Postman: via a local directory,
or by syncing Postman with your own fork of this repository. To sync Postman
with a GitHub repository, you must have a GitHub account, fork this repository,
and sign in to GitHub from within Postman.

Otherwise, either clone this repository via `git clone
https://github.com/IPQualityScore/OpenAPI.git` or download and unzip the ZIP file.

1. In Postman, in the workspace of your choice, click **Import**.

2. In the dialog window, click **files**.

3. Upload the `openapi.bundled.yaml` file within the unzipped or cloned directory.

4. Select **view import settings**.

5. In **Import Settings**, under **Folder organization**, select **Tags**. This
ensures that the requests in the collection will be organized by Postman more
legibly. Then click the back arrow.

6. Finally, click **Import** to import the collection into Postman.

You should now see a collection in Postman called IPQualityScore API. In order
to successfully make requests using the collection, you must have an API key. If
you do not have one, you can sign up for a free account at
[IPQualityScore.com](https://www.ipqualityscore.com/create-account).

Most of our requests allow the API key to be given in the URL path, as a query
parameter, in header, or in the body of a POST request. Regardless of which
option you choose, you will want to place the value of your API key in a
collection variable. Don't forget to click *save* once you've entered the key.

You will then be able to send requests. Please contact IPQualityScore support at
https://ipqualityscore.com if you need any further assistance.
