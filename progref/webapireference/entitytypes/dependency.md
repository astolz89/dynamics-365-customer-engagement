---
title: "Microsoft Dynamics 365 Customer Engagement dependency EntityType Reference | MicrosoftDocs"
ms.date: "2017-07-10"
ms.service: "crm-online"
ms.topic: "reference"
applies_to: 
  - "Dynamics 365 (online)"
ms.assetid: 8ea148ee-4327-42f4-923c-89099eba7566
author: "jimdaly"
ms.author: "jdaly"
manager: "jdaly"
meta-description: "Reference information about the Web API dependency entitytype."
---
# dependency EntityType
<table>
<tr><td><b>Description:</b></td><td>[!INCLUDE[./descriptions/dependency.md](./descriptions/dependency.md)]</td></tr>
<tr><td><b>Entity Set path:</b></td><td>[!include[current-web-api-base-uri.md](../includes/current-web-api-base-uri.md)]dependencies </td></tr>
<tr><td><b>Base Type:</b></td><td>[crmbaseentity EntityType](crmbaseentity.md)</td></tr>
<tr><td><b>Display Name:</b></td><td>Dependency</td></tr>
<tr><td><b>Primary Key:</b></td><td>dependencyid</td></tr>
<tr><td><b>Primary Name Attribute:</b></td><td>None</td></tr>
<tr><td><b>Operations supported:</b></td><td>GET</td></tr>
</table>
  
The dependency EntityType:
* Has no single-valued navigation properties.
* Has no collection-valued navigation properties.
* Has no functions or actions bound to it.
* Is not included in any solutions.

## Properties
Properties represent fields of data stored in the entity. Some properties are read-only. 


|Name|Type|Details|  
|----|----|-------|  
|dependencyid|Edm.Guid|**Display Name**: Dependency Identifier<br />**Description**: Unique identifier of a dependency.<br />Read-only<br />|
|dependencytype|Edm.Int32|**Display Name**: Dependency Type<br />**Description**: The dependency type of the dependency.<br />Read-only<br />**Default Options**:<br /><table><thead><td>**Value**</td><td>**Label**</td></thead><tbody><tr><td>0</td><td>None</td></tr><tr><td>1</td><td>Solution Internal</td></tr><tr><td>2</td><td>Published</td></tr><tr><td>4</td><td>Unpublished</td></tr><tbody></table>|
|dependentcomponentbasesolutionid|Edm.Guid|Read-only<br />|
|dependentcomponentobjectid|Edm.Guid|Read-only<br />|
|dependentcomponentparentid|Edm.Guid|Read-only<br />|
|dependentcomponenttype|Edm.Int32|Read-only<br />**Default Options**:<br /><table><thead><td>**Value**</td><td>**Label**</td></thead><tbody><tr><td>1</td><td>Entity</td></tr><tr><td>2</td><td>Attribute</td></tr><tr><td>3</td><td>Relationship</td></tr><tr><td>4</td><td>Attribute Picklist Value</td></tr><tr><td>5</td><td>Attribute Lookup Value</td></tr><tr><td>6</td><td>View Attribute</td></tr><tr><td>7</td><td>Localized Label</td></tr><tr><td>8</td><td>Relationship Extra Condition</td></tr><tr><td>9</td><td>Option Set</td></tr><tr><td>10</td><td>Entity Relationship</td></tr><tr><td>11</td><td>Entity Relationship Role</td></tr><tr><td>12</td><td>Entity Relationship Relationships</td></tr><tr><td>13</td><td>Managed Property</td></tr><tr><td>14</td><td>Entity Key</td></tr><tr><td>16</td><td>Privilege</td></tr><tr><td>17</td><td>PrivilegeObjectTypeCode</td></tr><tr><td>20</td><td>Role</td></tr><tr><td>21</td><td>Role Privilege</td></tr><tr><td>22</td><td>Display String</td></tr><tr><td>23</td><td>Display String Map</td></tr><tr><td>24</td><td>Form</td></tr><tr><td>25</td><td>Organization</td></tr><tr><td>26</td><td>Saved Query</td></tr><tr><td>29</td><td>Workflow</td></tr><tr><td>31</td><td>Report</td></tr><tr><td>32</td><td>Report Entity</td></tr><tr><td>33</td><td>Report Category</td></tr><tr><td>34</td><td>Report Visibility</td></tr><tr><td>35</td><td>Attachment</td></tr><tr><td>36</td><td>Email Template</td></tr><tr><td>37</td><td>Contract Template</td></tr><tr><td>38</td><td>KB Article Template</td></tr><tr><td>39</td><td>Mail Merge Template</td></tr><tr><td>44</td><td>Duplicate Rule</td></tr><tr><td>45</td><td>Duplicate Rule Condition</td></tr><tr><td>46</td><td>Entity Map</td></tr><tr><td>47</td><td>Attribute Map</td></tr><tr><td>48</td><td>Ribbon Command</td></tr><tr><td>49</td><td>Ribbon Context Group</td></tr><tr><td>50</td><td>Ribbon Customization</td></tr><tr><td>52</td><td>Ribbon Rule</td></tr><tr><td>53</td><td>Ribbon Tab To Command Map</td></tr><tr><td>55</td><td>Ribbon Diff</td></tr><tr><td>59</td><td>Saved Query Visualization</td></tr><tr><td>60</td><td>System Form</td></tr><tr><td>61</td><td>Web Resource</td></tr><tr><td>62</td><td>Site Map</td></tr><tr><td>63</td><td>Connection Role</td></tr><tr><td>64</td><td>Complex Control</td></tr><tr><td>70</td><td>Field Security Profile</td></tr><tr><td>71</td><td>Field Permission</td></tr><tr><td>90</td><td>Plugin Type</td></tr><tr><td>91</td><td>Plugin Assembly</td></tr><tr><td>92</td><td>SDK Message Processing Step</td></tr><tr><td>93</td><td>SDK Message Processing Step Image</td></tr><tr><td>95</td><td>Service Endpoint</td></tr><tr><td>150</td><td>Routing Rule</td></tr><tr><td>151</td><td>Routing Rule Item</td></tr><tr><td>152</td><td>SLA</td></tr><tr><td>153</td><td>SLA Item</td></tr><tr><td>154</td><td>Convert Rule</td></tr><tr><td>155</td><td>Convert Rule Item</td></tr><tr><td>65</td><td>Hierarchy Rule</td></tr><tr><td>161</td><td>Mobile Offline Profile</td></tr><tr><td>162</td><td>Mobile Offline Profile Item</td></tr><tr><td>165</td><td>Similarity Rule</td></tr><tr><td>66</td><td>Custom Control</td></tr><tr><td>68</td><td>Custom Control Default Config</td></tr><tr><td>166</td><td>Entity Data Source Mapping</td></tr><tr><td>201</td><td>SDKMessage</td></tr><tr><td>202</td><td>SDKMessageFilter</td></tr><tr><td>203</td><td>SdkMessagePair</td></tr><tr><td>204</td><td>SdkMessageRequest</td></tr><tr><td>205</td><td>SdkMessageRequestField</td></tr><tr><td>206</td><td>SdkMessageResponse</td></tr><tr><td>207</td><td>SdkMessageResponseField</td></tr><tr><td>210</td><td>WebWizard</td></tr><tr><td>18</td><td>Index</td></tr><tr><td>208</td><td>Import Map</td></tr><tbody></table>|
|requiredcomponentbasesolutionid|Edm.Guid|Read-only<br />|
|requiredcomponentintroducedversion|Edm.Double|Read-only<br />|
|requiredcomponentobjectid|Edm.Guid|Read-only<br />|
|requiredcomponentparentid|Edm.Guid|Read-only<br />|
|requiredcomponenttype|Edm.Int32|Read-only<br />**Default Options**:<br /><table><thead><td>**Value**</td><td>**Label**</td></thead><tbody><tr><td>1</td><td>Entity</td></tr><tr><td>2</td><td>Attribute</td></tr><tr><td>3</td><td>Relationship</td></tr><tr><td>4</td><td>Attribute Picklist Value</td></tr><tr><td>5</td><td>Attribute Lookup Value</td></tr><tr><td>6</td><td>View Attribute</td></tr><tr><td>7</td><td>Localized Label</td></tr><tr><td>8</td><td>Relationship Extra Condition</td></tr><tr><td>9</td><td>Option Set</td></tr><tr><td>10</td><td>Entity Relationship</td></tr><tr><td>11</td><td>Entity Relationship Role</td></tr><tr><td>12</td><td>Entity Relationship Relationships</td></tr><tr><td>13</td><td>Managed Property</td></tr><tr><td>14</td><td>Entity Key</td></tr><tr><td>16</td><td>Privilege</td></tr><tr><td>17</td><td>PrivilegeObjectTypeCode</td></tr><tr><td>20</td><td>Role</td></tr><tr><td>21</td><td>Role Privilege</td></tr><tr><td>22</td><td>Display String</td></tr><tr><td>23</td><td>Display String Map</td></tr><tr><td>24</td><td>Form</td></tr><tr><td>25</td><td>Organization</td></tr><tr><td>26</td><td>Saved Query</td></tr><tr><td>29</td><td>Workflow</td></tr><tr><td>31</td><td>Report</td></tr><tr><td>32</td><td>Report Entity</td></tr><tr><td>33</td><td>Report Category</td></tr><tr><td>34</td><td>Report Visibility</td></tr><tr><td>35</td><td>Attachment</td></tr><tr><td>36</td><td>Email Template</td></tr><tr><td>37</td><td>Contract Template</td></tr><tr><td>38</td><td>KB Article Template</td></tr><tr><td>39</td><td>Mail Merge Template</td></tr><tr><td>44</td><td>Duplicate Rule</td></tr><tr><td>45</td><td>Duplicate Rule Condition</td></tr><tr><td>46</td><td>Entity Map</td></tr><tr><td>47</td><td>Attribute Map</td></tr><tr><td>48</td><td>Ribbon Command</td></tr><tr><td>49</td><td>Ribbon Context Group</td></tr><tr><td>50</td><td>Ribbon Customization</td></tr><tr><td>52</td><td>Ribbon Rule</td></tr><tr><td>53</td><td>Ribbon Tab To Command Map</td></tr><tr><td>55</td><td>Ribbon Diff</td></tr><tr><td>59</td><td>Saved Query Visualization</td></tr><tr><td>60</td><td>System Form</td></tr><tr><td>61</td><td>Web Resource</td></tr><tr><td>62</td><td>Site Map</td></tr><tr><td>63</td><td>Connection Role</td></tr><tr><td>64</td><td>Complex Control</td></tr><tr><td>70</td><td>Field Security Profile</td></tr><tr><td>71</td><td>Field Permission</td></tr><tr><td>90</td><td>Plugin Type</td></tr><tr><td>91</td><td>Plugin Assembly</td></tr><tr><td>92</td><td>SDK Message Processing Step</td></tr><tr><td>93</td><td>SDK Message Processing Step Image</td></tr><tr><td>95</td><td>Service Endpoint</td></tr><tr><td>150</td><td>Routing Rule</td></tr><tr><td>151</td><td>Routing Rule Item</td></tr><tr><td>152</td><td>SLA</td></tr><tr><td>153</td><td>SLA Item</td></tr><tr><td>154</td><td>Convert Rule</td></tr><tr><td>155</td><td>Convert Rule Item</td></tr><tr><td>65</td><td>Hierarchy Rule</td></tr><tr><td>161</td><td>Mobile Offline Profile</td></tr><tr><td>162</td><td>Mobile Offline Profile Item</td></tr><tr><td>165</td><td>Similarity Rule</td></tr><tr><td>66</td><td>Custom Control</td></tr><tr><td>68</td><td>Custom Control Default Config</td></tr><tr><td>166</td><td>Entity Data Source Mapping</td></tr><tr><td>201</td><td>SDKMessage</td></tr><tr><td>202</td><td>SDKMessageFilter</td></tr><tr><td>203</td><td>SdkMessagePair</td></tr><tr><td>204</td><td>SdkMessageRequest</td></tr><tr><td>205</td><td>SdkMessageRequestField</td></tr><tr><td>206</td><td>SdkMessageResponse</td></tr><tr><td>207</td><td>SdkMessageResponseField</td></tr><tr><td>210</td><td>WebWizard</td></tr><tr><td>18</td><td>Index</td></tr><tr><td>208</td><td>Import Map</td></tr><tbody></table>|

## Lookup Properties
Lookup properties are read-only, computed properties which contain entity primary key Edm.Guid data for one or more corresponding single-valued navigation properties. More information: [Lookup properties](https://msdn.microsoft.com/library/mt607990.aspx#bkmk_lookupProperties) and [Retrieve data about lookup properties](https://msdn.microsoft.com/library/gg334767.aspx#bkmk_lookupProperty).


|Name|Single-valued navigation property|Description|  
|----|---------------------------------|-----------|  
|_dependentcomponentnodeid_value||Unique identifier of the dependent component's node.|
|_requiredcomponentnodeid_value||Unique identifier of the required component's node|

## Operations
The following operations can be used with the dependency entity type.


|Name|Binding|Description|  
|----|-------|-----------|  
|[RetrieveDependenciesForDelete Function](../functions/retrievedependenciesfordelete.md)|Not Bound|[!INCLUDE[../functions/descriptions/retrievedependenciesfordelete.md](../functions/descriptions/retrievedependenciesfordelete.md)]|  
|[RetrieveDependenciesForUninstall Function](../functions/retrievedependenciesforuninstall.md)|Not Bound|[!INCLUDE[../functions/descriptions/retrievedependenciesforuninstall.md](../functions/descriptions/retrievedependenciesforuninstall.md)]|  
|[RetrieveDependentComponents Function](../functions/retrievedependentcomponents.md)|Not Bound|[!INCLUDE[../functions/descriptions/retrievedependentcomponents.md](../functions/descriptions/retrievedependentcomponents.md)]|  
|[RetrieveMissingDependencies Function](../functions/retrievemissingdependencies.md)|Not Bound|[!INCLUDE[../functions/descriptions/retrievemissingdependencies.md](../functions/descriptions/retrievemissingdependencies.md)]|  
|[RetrieveRequiredComponents Function](../functions/retrieverequiredcomponents.md)|Not Bound|[!INCLUDE[../functions/descriptions/retrieverequiredcomponents.md](../functions/descriptions/retrieverequiredcomponents.md)]|    

[!INCLUDE[./remarks/dependency.md](./remarks/dependency.md)]

### See also  
 [Use the Microsoft Dynamics CRM Web API](https://msdn.microsoft.com/library/mt593051.aspx)<br />
 [Web API EntityType Reference](../entitytypereference.md)<br />
 [Web API Action Reference](../actionreference.md)<br />
 [Web API Function Reference](../functionreference.md)<br />
 [Web API Query Function Reference](../queryfunctionreference.md)<br />
 [Web API EnumType Reference](../enumtypereference.md)<br />
 [Web API ComplexType Reference](../complextypereference.md)<br />
 [Web API Metadata EntityType Reference](../entitytypereference.md)<br />
 [Web API Solutions Reference](../solutionreference.md)<br />