Use Case:
---------

Aero Technica Solutions (ATS) is creating new systems to manage the usage of its new drone product line. These drones have specific permissible fly zones depending on the country in which they are being operated. Additionally, ATS offers an insurance policy for its drones, covering various scenarios including loss or damage. To ensure both customer satisfaction and legal compliance, your team needs to implement a system that will aid customers in finding the zones in which they can legally operate their drones. They also need to be able to submit an insurance claim, if necessary. Finally, third parties need to be able to access a database of drone operators to determine if a certain user is certified to operate a specific drone.


Business Requirements:
----------------------
----------------------

Find the Permissible Fly Zone:
------------------------------

ATS aims to enhance the user experience on its website by providing information about fly zones of its products based on the product code and the country where it’s intended to be operated. This requires the development and integration of a custom API that will fetch product family for the product code and process data to determine the product's fly zone for a country. The Product Geo Mapping custom metadata type configures the mapping of the permissible fly zones between the Product Family (API Name Family) field in the Product object and a country code. Countries covered for this requirement are US, AU, DE, and FR. You may assume that if a country value is specified, it will be one of these.

The system automatically detects the country and sets that value in the request headers. Design and implement an Apex REST service class (named ProductZoningService) configured to use the endpoint path ProductZoning with wildcard. The API should accept CountryCode as a custom header and the ProductCode as an input parameter to return a string value of the permissible fly zones or appropriate response mentioned below. If CountryCode is not found in the request headers, the implementation should default the country code to US.

Tip: To understand the mapping between the Family field and a country code, review the Product Geo Mapping custom metadata type and the Product object in Setup, and examine the sample records.

Test this API by implementing a comprehensive test class that covers various scenarios to ensure robustness and accuracy. Your solution should account for the three specific outcomes.

Permissible Fly Zones: Test with valid product codes and countries that should return specific permissible fly zones. The API should accurately return [Permissible_Fly_Zone__c] zones only where Permissible_Fly_Zone__c is the value associated with the input.
Check with Local Authorities: Include scenarios where the outcome instructs the user to confirm with local authorities. This applies to situations where a country in the system does not have defined regulations for a particular product family. Ensure the API provides a response of Confirm with the local authorities under the correct conditions.
Invalid or Missing Product Code: Test with scenarios where the ProductCode is either missing or does not exist in the system. The API should handle these cases and return ProductCode is missing or doesn't exist in response.
Tip: Test the API using Postman or a similar tool to verify the above outcomes. Do not create additional Product Geo Mapping custom metadata type records for testing purposes.

Aim for a test class that achieves at least 90% code coverage, ensuring that the scenarios are tested thoroughly. This includes positive tests with expected results, as well as negative tests handling errors or exceptional cases.


Report a Lost Device:
--------------------

An unfortunate reality of flying drones is that they can sometimes be lost or damaged. To address this issue, your company offers various insurance coverage plans with each drone purchase. Develop an Apex REST service class named AssetService with an endpoint path lost. This API plays a crucial role in reporting lost devices and creating customer insurance claims efficiently. The assetIdentifier field from the JSON body should be passed as a string parameter to the reportLostDevice method.

It's important to thoroughly test this system to ensure that it accurately assesses insurance coverage and handles claims reliably. When a device is reported lost, ensure the service method in the AssetService class updates the asset status to 'Lost' and creates a claim record if there is an active comprehensive insurance coverage. The method should return a string value of claim number (API Name Claim__c.Name) or a message described below. Testing the claim system involves simulating real-world scenarios to ensure the API responds correctly under various conditions.

Invalid Asset Identifier: Test with scenarios where the assetIdentifier is either missing or does not exist in the system. The API should handle these cases and return No device found. in the response.
Claim Number: Test with an asset record which has an active comprehensive insurance coverage, and returns the claim number (API Name Claim__c.Name).
Claim Already Filed: If there's already a claim of type Loss, the method should return the message [Claim number] already filed.
No Coverage: If a device is not insured, the API should update the status to 'Lost' and return No coverage. Asset status adjusted to Lost.
Tip: Test the API using Postman or a similar tool to verify the above outcomes.

For each of these use cases, aim to achieve at least 80% code coverage in your test class. Ensure that your tests accurately reflect these scenarios, including both the positive outcomes (where claims are filed successfully) and negative outcomes (such as no asset found or insurance coverage being inadequate). This ensures the API can handle all possible cases effectively.


Confirm Certification Status:
-----------------------------


Many drone operators use their drones for commercial photography and filming. It’s important for their clients (in this case, realtors) to ensure that the drone operators they hire are properly licensed to operate drones. To help them find this information, you need to set up a system that allows third parties to verify the certification status of registered drone operators. This system should provide a straightforward way for realtors to confirm whether a drone operator holds an active license, ensuring compliance and safety standards are met.

In this challenge, your task is to develop an Apex class named CredentialVerificationService that functions as a SOAP web service, facilitating the verification of drone operator certifications. Define a method called verifyCredential for which the input is the Contact's Last Name (API Name LastName) and a unique contact certification name (API Name Contact_Certification__c.Name), linking the Contact and Certification records. The service is expected to return a response of type string. Depending on isActive__c (API Name Contact_Certification__c.isActive__c) value of the Contact Certification record, the response should indicate whether the certification is Valid or Needs Renewal. The API should also consider scenarios where the Contact Last Name and the contact certification name doesn't exist. The response in such a case should be No record found.

Tip: You can export the WSDL and test the API using Postman or a similar tool to verify the outcomes.

Aim for at least 90% code coverage in your test class, ensuring comprehensive testing across various scenarios, including both positive and negative outcomes. This includes tests for active certifications, inactive certifications, and handling of invalid or incomplete input.
