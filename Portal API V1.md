![Scope Technologies](https://cloud.githubusercontent.com/assets/3167692/9407536/cfc140b0-4812-11e5-96aa-21fa51a0cb0f.png)

Portal API
==========

To use Portal API properly developers must understand basic concept of OData.
Check http://www.asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v4/ for OData tutorials.

All action names and "verbs" in this document is relevant to client (not server).
 
OData Navigation Properties
-------------

Navigation properties is used to expand source object

| Source Type | Property |  Target Type | Description |
|---|---|---|---|
|Accident	        |	PolicyVehicleUnit	|  |  |
|PolicyVehicleUnit	|	Accidents	|
|PolicyVehicleUnit	|	Drivers	        |
|PolicyVehicleUnit	|	Place	|
|PolicyVehicleUnit	|	Policy	|
|PolicyVehicleUnit	|	PolicyVehicleUnitUsers	|
|PolicyVehicleUnit	|	Shares	|
|PolicyVehicleUnit	|	Unit	|
|PolicyVehicleUnit	|	UnitReplacements	|
|PolicyVehicleUnit	|	Vehicle	|
|Driver	|	ContactPerson	|
|Driver	|	PolicyVehicleUnit	|
|ContactPerson	|	Customers	|
|ContactPerson	|	DeliveryPolicies	|
|ContactPerson	|	Drivers	|
|ContactPerson	|	Users	|
|Customer	|	ContactPerson	|
|Customer	|	Policies	|
|Policy	|	Customer	|
|Policy	|	CustomPolicyFields	|
|Policy	|	DeliveryPerson	|
|Policy	|	PolicyProvider	|
|Policy	|	PolicyVehicleUnits	|
|CustomPolicyField	|	Policy	|
|PolicyProvider	|	Policies	|
|User	|	AcceptedShares	|
|User	|	ContactPerson	|
|User	|	IssuedShares	|
|User	|	Pages	|
|User	|	PastPasswords	|
|User	|	PolicyUploads	|
|User	|	PolicyVehicleUnitUsers	|
|Share	|	Donor	|
|Share	|	PolicyVehicleUnit	|
|Share	|	Recipient	|
|Page	|	User	|
|PastPassword	|	User	|
|PolicyUpload	|	User	|
|PolicyVehicleUnitUser	|	PolicyVehicleUnit	|
|PolicyVehicleUnitUser	|	User	|
|Place	|	Children	|   Place| 
|Place	|	Parent	|   Place| 
|Place	|	PolicyVehicleUnits	|
|Unit	|	PolicyVehicleUnits	|
|Unit	|	UnitReplacements	|
|UnitReplacement	|	PolicyVehicleUnit	|
|UnitReplacement	|	Unit	|
|Vehicle	|	PolicyVehicleUnits	|
|NotificationField	|	Notification	|
|Notification	|	NotificationFields	|


PolicyVehicleUnit Concept
-------------------------
PolicyVehicleUnit is  intersection between Policy Vehicle and Unit. PolicyVehicleUnit item shows that particular Vehicle is or was linked to particular Policy and particular Unit is or was installed in that vehicle in particular period of time. Unit is nullable in PolicyVehicleUnit 

Internalization
------------------
To get response in specific language add X-UI-Language header to request, otherwise default language will be used. 
Sample:

    X-UI-Language: en
 
Token
-----
Access to Portal API is controlled with Bearer Authentication. Client needs to obtain authentication token then add it to each request

###Request

| **** | **** |
|---|---|
| URL | ~/token |
| Method | POST |
| Content-Type | application/x-www-form-urlencoded  | 
| Payload | grant_type=password&username=**UserName**&password=**Password**  | 

| Parameters |  |
|---|---|
| UserName | Username to log in  |
| Password | Password to log in, ensure sent password is "urlencoded" | 

###Response

**Content-Type** application/json;charset=UTF-8 

**Sample**

    {
    	"access_token": "BHM1jhi==", // This value must be used to authenticate
    	"token_type": "bearer",
    	"expires_in": 1209599,
    	"userId": "106",
    	"userName": "Admin",
    	"role": "Administrator",
    	"userDisplayName": "Admin",
    	".issued": "Thu, 06 Aug 2015 11:19:05 GMT",
    	".expires": "Thu, 20 Aug 2015 11:19:05 GMT"
    }

Further requests which require caller to be authenticated must be completed with access_token value in Authorization header.

> Authorization: Bearer BHM1jhi==


Classifiers (Enumerations)
-----------
All classifiers in the system is declared inside **Blackjack.Models.Classifiers** namespace, use it for OData requests.  
Following classifiers (enumerations) is used by the systems:

<a name="DateSpanType"></a>

    /// <summary>
    /// Predefined date spans
    /// </summary>
    public enum DateSpanType
    {
        Day = 1,
        Week = 2,
        Month = 3,
        Quarter = 4,
        HalfYear = 5,
        Year = 6,
        Custom = 7
    }

<a name="DeliveryMethod"></a>
  
    /// <summary>
    /// MHub unit delivery methods from insurer related warehouse to end user 
    /// </summary>
    public enum DeliveryMethod
    {
        ByHand = 1,
        ByMessenger = 2,
        ByMail = 3
    }

<a name="DeviceType"></a>
  
    /// <summary>
    /// Device type by connection type
    /// </summary>
    public enum DeviceType
    {
        MHubObd = 1, // connects to OBD port
        MHub846 = 2, // connects to Battery
    }

<a name="MotorType"></a>
  
    /// <summary>
    /// Fuel used to move vehicle
    /// </summary>
    public enum MotorType
    {
        Unknown = 0,
        Petrol = 1, 
        Diesel = 2, 
        Gas = 3,
        Hybrid = 4,
        Electric = 5
    }

<a name="NotificationType"></a>
  
    /// <summary>
    /// Types of notification
    /// </summary>
    public enum NotificationType
    {
        ContactForm = 1,
        RegistrationRequest = 2,
        RegistrationCompleted = 3,
        FirstOverdue = 4,
        SecondOverdue = 5,
        ThirdOverdue = 6,
        ResetPassword = 7,
        GeneratedPassword = 8,
        FirstTripCompletion = 9,
        PluggedOut = 10,
        PluggedIn = 11,
        DifferentVehicleIdentified = 12,
        NeverReported = 13,
        NotReporting = 14,
        NoImAlive = 15,
        DeviceToBeReturned = 16,
        DeviceReturned = 17,
        ShareInvitation = 18,
        PolicyRenewal = 19,
        UnitTransfer = 20,
        OutOfProvince = 21,
        OutOfRegion = 22,
        OutOfCountry = 23,
        FirstTripInvitation = 24,
        ShareAccepted = 25
    }    

<a name="Permissions"></a>
  
    /// <summary>
    /// User's vehicle access permissions
    /// </summary>
    [Flags]
    public enum Permissions
    {
        View = 0x0001, 
        Edit = 0x0002,
        Share = 0x0004
    }

<a name="PolicyStatus"></a>
       
    /// <summary>
    /// Policy Status
    /// </summary>
    public enum PolicyStatus
    {
        Ordered = 1,
        Shipped = 2,
        Registered = 3,
        Activated = 4,
        ActivatedAndRegistered = 5,
        Deactivated = 6
    }    

<a name="UserAction"></a>
         
    /// <summary>
    /// Logged user action
    /// </summary>
    public enum UserAction
    {
        LogIn = 1,
        LogOut = 2,
        PasswordChange = 3,
        PasswordResetRequest = 4,
        PasswordResetComplete = 5,
        PasswordResetCancel = 6,
        ProfileUpdate = 7,
        LogbookExport = 8,
        AccidentDownload = 9,
        RegistrationComplete = 10
    }

<a name="VehicleColor"></a>
  
    /// <summary>
    /// Colors of Vehicle
    /// </summary>
    public enum VehicleColor
    {
        Black = 0x000000,   // decimal value = 0
        Gray = 0x808080,    // decimal value = 8421504
        Silver = 0xC0C0C0,  // decimal value = 12632256
        White = 0xFFFFFF,   // decimal value = 16777215
        Navy = 0x000080,    // decimal value = 128
        Blue = 0x60CC93,    // decimal value = 6343827
        Teal = 0x008080,    // decimal value = 32896
        Aqua = 0x0000FF,    // decimal value = 255
        Olive = 0x808000,   // decimal value = 8421376
        Green = 0x008000,   // decimal value = 32768
        Lime = 0x00FF00,    // decimal value = 65280
        Purple = 0x800080,  // decimal value = 8388736
        Magenta = 0xFF00FF, // decimal value = 16711935
        Red = 0xFF0000,     // decimal value = 16711680
        Maroon = 0x800000,  // decimal value = 8388608
        Yellow = 0xFFFF00   // decimal value = 16776960
    }
    
Some of classifiers with localized descriptions can be retrieved via API in a following format:

    [
      {
        "Key": 1, //ID of classifier value
        "Value": "Value 1" //Textual representation of classifier value
      },
      {
        "Key": 2,
        "Value": "Value 2"
      },
      ...
    ]

Following classifiers with localized descriptions can be retrieved via API:

###MotorTypes
| **** | **** |
|---|---|
| Description | Returns MotorTypes |
| URL | ~/api/Classifiers/MotorTypes|
| Method | GET |
| Authorize | no authorization |

###Colors
| **** | **** |
|---|---|
| Description | Returns vehicle colors|
| URL | ~/api/Classifiers/Colors|
| Method | GET |
| Authorize | no authorization |

###NotificationTypes
| **** | **** |
|---|---|
| Description | Returns Notification Types|
| URL | ~/api/Classifiers/NotificationTypes|
| Method | GET |
| Authorize | no authorization |

Account - Change Password
-------
| **** | **** |
|---|---|
| URL | ~/api/Account/ChangePassword |
| Method | Post |
| Authorize | Administrator, User  | 

###Request Payload Sample:

    {
    "OldPassword":"0!dP@ssw0rd", //required
    "newPassword":"NewP@ssw0rd", //required
    "confirmPassword":"NewP@ssw0rd" //required, must be same as "newPassword"
    }

###Response 
Status Code ( 200 OK or 500 Internal Server Error)

Account - Register with Policy
-------
| **** | **** |
|---|---|
| Description | Register Insured Person with customer/policy number and unit number|
| URL | ~/api/Account/RegisterWithPolicy |
| Method | POST |
| Authorize | Anonymous | 
    
###Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| UserName | string | yes | |
| Password | string | yes | |
| PolicyOrCustomerNumber | string | yes | |
| SerialNumber| string | yes | |
| AgreementAccepted| bool | yes | |  

###Response 
Integer - PolicyVehicleUnit ID 

Account - Register with Share
-------
| **** | **** |
|---|---|
| Description | Register Insured Person with share code |
| URL | ~/api/Account/RegisterWithShare |
| Method | POST |
| Authorize | Anonymous | 

###Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| SharedCode| string | yes | |
| UserName | string | yes | |
| Password | string | yes | |
| AgreementAccepted| bool | yes | | 

###Response 
Integer - PolicyVehicleUnit ID 

 
Get Accidents Reports in PDF 
---------
  
###Request

| **** | **** |
|---|---|
| URL | ~/api/Accidents/ReportInPdf |
| Method | GET |

| Parameters |  |
|---|---|
| id | ID of accident  |

###Response

**Content-Type** text/plain; charset=utf-8
**Payload** Base64 String containing PDF File


Accidents OData
-------------------------
| **** | **** |
|---|---|
| URL | ~/odata/Accidents |
| Available Methods | GET, DELETE |
| Parameters | OData URI parameters  | 
| Authorize | Administrator  | 
| Max Expansion Depth| 3 | 

###Accident Object

| Propety | Type | Nullable | Store Pattern | Max Length | Fixed Length | Unicode | Precision | Scale | Description |
|---|---|---|---|---|---|---|---|---|---|
|	Id	|	Int32	|	FALSE	|	Identity	|		|		|		|		|		|
|	PolicyVehicleUnitId	|	Int32	|	FALSE	|		|		|		|		|		|		|
|	DateCreated	|	DateTimeOffset	|	FALSE	|		|		|		|		|		|		|
|	AccidentId	|	Int64	|	FALSE	|		|		|		|		|		|		|
|	UnitSerialNumber	|	String	|	FALSE	|		|	20	|	FALSE	|	TRUE	|		|		|
|	EventDateTime	|	DateTimeOffset	|	FALSE	|		|		|		|		|		|		|
|	EventLocationGeoDataLatitude	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	EventLocationGeoDataLongitude	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	GeoDataLatitudeXBeforeAccident	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	GeoDataLongitudeXBeforeAccident	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	GeoDataLatitude45SecAfterAccident	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	GeoDataLongitude45SecAfterAccident	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	DrivingDirection	|	Int32	|		|		|		|		|		|		|		|
|	SpeedAtEventTime	|	Decimal	|		|		|		|		|		|	18	|	2	|
|	MaxGForce	|	Decimal	|	FALSE	|		|		|		|		|	18	|	2	|
|	ImpactZones	|	String	|	FALSE	|		|	50	|	FALSE	|	TRUE	|		|		|
|	ImpactAngle	|	Int32	|	FALSE	|		|		|		|		|		|		|
|	ImpactSeverity	|	String	|	FALSE	|		|	50	|	FALSE	|	TRUE	|		|		|
|	ImpactMagnitude	|	String	|	FALSE	|		|	50	|	FALSE	|	TRUE	|		|		|

CSV
---
###Policy
<span class="todo">FUTURE CHANGE: Pluralize name</span>

| **** | **** |
|---|---|
| Description | Get CSV file containing all Policies |
| URL | ~/api/Csv/Policy|
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | text/csv |

###Trip
<span class="todo">FUTURE CHANGE: Pluralize name</span>

| **** | **** |
|---|---|
| Description | Get CSV file containing filtered set of trips |
| URL | ~/api/Csv/Trip |
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | text/csv |

**Request Parameters**

| Name | Type | Description |
|---|---|---|
| pvu | int | PolicyVehicleUnit ID |
| bd | DateTime | Filter start date |
| ed | DateTime | Filter end date  |

<span class="todo">FUTURE CHANGE: Change parameters names</span> 

###Accident
<span class="todo">FUTURE CHANGE: Pluralize name</span>

| **** | **** |
|---|---|
| Description | Get CSV file containing all Accidents |
| URL | ~/api/Csv/Accident |
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | text/csv |


ECall - Send Accident information
---------------

| **** | **** |
|---|---|
| Description | Send accident description to Blackjack DB |
| URL | ~/api/ecall |
| Method | POST |
| Authorize | Administrator | 

###Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| AccidentId | int | yes | MZone accident ID |
| UnitSerialNumber | string | yes |
| EventDateTime | DateTimeOffset | yes | Date and time of event in UTC |
| EventLocationGeoDataLatitude | decimal | no | |
| EventLocationGeoDataLongitude | decimal | no | |
| GeoDataLatitudeXBeforeAccident | decimal | no | |
| GeoDataLongitudeXBeforeAccident | decimal | no | |
| GeoDataLatitude45SecAfterAccident | decimal | no | |
| GeoDataLongitude45SecAfterAccident | decimal | no | |
| DrivingDirection | int | no | Driving direction angle |
| SpeedAtEventTime | decimal | no | |
| ImpactZones | string | yes | Comma-separated list of impact zones; Zones: F, FL, L, BL, B, BR, R, FR |
| ImpactAngle | int | yes | |
| ImpactSeverity | string | yes | Textual description of impact severity e.g. medium or high |
| ImpactMagnitude | string | yes | Textual representation of decimal value of impact magnitude |
| MaxGForce | decimal | yes | |
| PdfReport | byte[] | no | PDF Report will generated if empty field is received |

###Response 
Integer - PolicyVehicleUnit ID 

Drivers
-------
<span class="todo">FUTURE CHANGE: Understand why it is needed</span>

ImageBrowser
------------
<span class="todo">FUTURE CHANGE: Development postponed, API is not ready yet</span> 

InsuredPersons - Self
--------------
| **** | **** |
|---|---|
| Description |Get Insured Person Info |
| URL | ~/api/InsuredPersons/Self |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

###Response   

| Property | Type| Description |
|---|---|---|
| UserId| int | |
| UserName| string | |
| IsDisabled| bool | |
| Description| string | always empty |
| FirstName| string | always empty |  
| LastName| string | always empty |   
| Email| string | always empty |   
| LanguageCode| string | e.g.: en-US  |
| CurrencyCode| string | |
| CurrencySymbol| string | |
| DistanceMeasure| string | |
| FluidMeasure| string | |
| SpeedMeasure| string | |
| DefaultScore| int | Score to show if score is not available  |
| InsuredVehicles | Array of [User's Insured Vehicles](#InsuredVehicleUserData) | List of accessible vehicles |  


<a name="InsuredVehicleUserData"></a>
###Insured Vehicle Object

| Property | Type| Description |
|---|---|---|
| Permissions | [Permissions](#Permissions) |  |
| AlertLevel1 | bool | Receive out of country notifications generated by vehicle |  
| AlertLevel2 | bool | Receive out of region notifications generated by vehicle | 
| AlertLevel3 | bool | Receive out of province notifications generated by vehicle  |
| FriendlyName | string | Current user specific, friendly name of vehicle |   
| FullFriendlyName| string | FriendlyName with validity period if PolicyVehicleUnit is expired  |
| PolicyVehicleUnitId |  int |  PolicyVehicleUnit ID |
| PolicyId| int |  Policy ID |
| PolicyNumber| string | | 
| PolicyCustomer.Number | string | | 
| PolicyCustomer.Name| string | |  
| PolicyCustomer.Contacts| Array | always null |
| PolicyStartDate| DateTimeOffset | | 
| PolicyTerminationDate | DateTimeOffset |  |  | 
| Vehicle.Id | int |  |  |
| Vehicle.VIN | string | VIN or Insurer specific unique vehicle identifier| 
| Vehicle.HSN | string | Herstellerschlüsselnummer - germany specific vehicle make/model identifier  |
| Vehicle.TSN | string | Typschlüsselnummer - germany specific vehicle make/model identifier |
| Vehicle.MakeModelCode | string | Insurer specific make model code | 
| Vehicle.LicensePlate | string | | 
| Vehicle.Color |  string | | 
| Vehicle.Make |  string | | 
| Vehicle.Model |  string | | 
| Vehicle.YearOfInitialRegistration |  int | 
| Vehicle.MotorType |  string | | 
| UnitId| int | | 
| UnitSerialNumber|  string | | 
| UnitAssignmentDate| DateTimeOffset | | 
| UnitActivationDate|  null|  |
| DeviceType| [DeviceType](#DeviceType) |  |
| IsExpired|  bool | |

<span class="todo">FUTURE CHANGE: Remove VehicleList from InsuredPersonsController</span>

InsuredPersons - Stats
--------------
| **** | **** |
|---|---|
| Description | Get driving statistics for insured person |
| URL | ~/api/InsuredPersons/Stats |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | PolicyVehicleUnit ID |
| span | string | yes | One of [DateSpan values](#DateSpanType) |
| clientDateTime | DateTimeOffset| no | Client's local time to adjust DateSpan filter |
| start | DateTimeOffset| no | Custom filter date start works when [DateSpan Custom](#DateSpanType) passed |
| end | DateTimeOffset| no |  Custom filter date end works works [DateSpan Custom](#DateSpanType) passed  |

**Response Object**

| Type | Name | Description |
|---|---|---|
|  int | DrivingTime |  |
|  double | DrivingDistance |  |
|  double | MaxSpeed |  |
|  int | NumberOfExceptions |  |
|  double | Score |  |
|  DateTimeOffset | FilterBegin |  |
|  DateTimeOffset | FilterEnd |  |
|  double | MinScore |  |
|  double | MaxScore |  |

InsuredPersons - Feedback
--------------
| **** | **** |
|---|---|
| Description | Get driving feedback for insured person |
| URL | ~/api/InsuredPersons/Feedback |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | PolicyVehicleUnit ID |
| span | string | yes | One of [DateSpan values](#DateSpanType) |
| clientDateTime | DateTimeOffset| no | Client's local time to adjust DateSpan filter |
| start | DateTimeOffset| no | Custom filter date start works when [DateSpan Custom](#DateSpanType) passed |
| end | DateTimeOffset| no |  Custom filter date end works works [DateSpan Custom](#DateSpanType) passed  |

**Response Object**

| Type | Name | Description |
|---|---|---|
|  int | ErrorCode | Error code (hopefully 0) |
|  string | ErrorMessage |  Error text  (hopefully null) |
|  List of [Driving Feedback Items](#DrpDrivingFeedbackItem) | Items | 

<a name="DrpDrivingFeedbackItem"></a>
**Driving Feedback Item**

| Type | Name | Description |
|---|---|---|
| string | Title | Measurement feedback is related to | 
| string | Key | Key of feedback text |
| string | Description | Feedback text |
| double | RangeMin | Minimal feedback value valid for returned feedback text |
| double | RangeMax | Maximum feedback value valid for returned feedback text |
| double | Value | Value feedback is related to  |

InsuredPersons - Events
--------------
| **** | **** |
|---|---|
| Description | Get driver behavior events |
| URL | ~/api/InsuredPersons/Events |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | PolicyVehicleUnit ID |
| beginDate | DateTimeOffset| yes  | Filter date start |
| endDate| DateTimeOffset | yes |  Filter date end |
| pageIndex | int | no | Page index for paging |
| pageSize | int | no |  Page size for paging |

Paging works only if both pageIndex and pageSize is defined.

**Response Object**

| Type | Name | Description |
|---|---|---|
|  int | Count | Total count of events |
|  bool | HasMoreResults |  Represents if page is last or not |
|  List of [Event Descriptions](#EventDescription) | Data |  |

<a name="EventDescription"></a>
**Event Description**

| Type | Name | Description |
|---|---|---|
|  Int64 | Id | Event ID |
|  string | UnitId | Unit ID | 
|  int | EventTypeId | Event Type ID |  
|  string | EventTypeDescription | |
|  DateTime | LocalTimestamp | |
|  List<decimal?> | Position | Longitude and Latitude in Decimal degrees (DD) format |
|  int | Speed | | 
|  string | UnitOfDistanceCode | Measurement of speed e.g: "km" |

InsuredPersons - Last Known Position
--------------
| **** | **** |
|---|---|
| Description | Get last known position of vehicle |
| URL | ~/api/InsuredPersons/LastKnownPosition |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | PolicyVehicleUnit ID |

**Response Object**

| Type | Name | Description |
|---|---|---|
|  DateTime | LocalTimestamp | |
|  Guid | Id | MZone Vehicle ID | 
|  string | Description | MZone Vehicle name |
|  string | UnitId | Unit ID | 
|  List<decimal?> | Position | Longitude and Latitude in Decimal degrees (DD) format |
|  string | Location | Location description |

Log Entries OData
-------------------------
| **** | **** |
|---|---|
| URL | ~/odata/LogEntries |
| Available Methods | GET, POST |
| Parameters | OData URI parameters  | 
| Authorize | Administrator  | 
| Max Expansion Depth | 0 | 

###Log Entry Object

| Propety | Type | Nullable | Store Pattern | Max Length | Fixed Length | Unicode | Precision | Scale | Description |
|---|---|---|---|---|---|---|---|---|---|
|	Id	|	Int64	|	FALSE	|	Identity	|		|		|		|		|		|
|	Timestamp	|	DateTimeOffset	|	FALSE	|		|		|		|		|		|		|
|	Level	|	String	|	FALSE	|		|	20	|	FALSE	|	TRUE	|		|		|
|	Logger	|	String	|	FALSE	|		|	512	|	FALSE	|	TRUE	|		|		|
|	Message	|	String	|	FALSE	|		|	Max	|	FALSE	|	TRUE	|		|		|
|	RawUrl	|	String	|		|		|	512	|	FALSE	|	TRUE	|		|		|
|	User	|	String	|		|		|	64	|	FALSE	|	TRUE	|		|		|
|	UserId	|	Int32	|		|		|		|		|		|		|		|
|	RemoteIP	|	String	|		|		|	64	|	FALSE	|	TRUE	|		|		|
|	MachineName	|	String	|	FALSE	|		|	256	|	FALSE	|	TRUE	|		|		|
|	Exception	|	String	|		|		|	Max	|	FALSE	|	TRUE	|		|		|


Notifications - Active
--------------
Returns all notifications which were created for authenticated user since last call of [Notifications - Mark Read](Notifications-MarkRead)  for that user.

| **** | **** |
|---|---|
| URL | ~/api/Notifications/Feedback |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Array of JSON objects |

**Response Object (Item in the array) **

| Type | Name | Description |
|---|---|---|
|  int | NotificationId | | 
|  NotificationType | [NotificationType](NotificationType) |  | 
|  DateTimeOffset | NotificationDate | | 
|  string | TranslatedNotificationType | Localized notification type name |
|  string | FormattedNotificationTypeDescription | Formatted notification type description  |
|  List of [Notification Fields](NotificationFieldViewModel1) | NotificationFields | | 

<a name="NotificationFieldViewModel1"></a>
**Notification Field**

| Type | Name | Description |
|---|---|---|
| string | Name | | 
| string | Value | |

<a name="Notifications-MarkRead"></a>
Notifications - Mark Read
--------------
Marks all existing notifications for authenticated user as read.

| **** | **** |
|---|---|
| URL | ~/api/Notifications/MarkRead|
| Method | POST |
| Authorize | Insured Person |
| Response | HTTP Status |

Notifications - Last
--------------
Returns all notifications for authenticated user which were created within last 24 hours.

| **** | **** |
|---|---|
| URL | ~/api/Notifications/Last |
| Method | GET |
| Authorize | Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Array of JSON objects |

**Response Object (Item in the array) **

| Type | Name | Description |
|---|---|---|
|  int | NotificationId | | 
|  NotificationType | [NotificationType](NotificationType) |  | 
|  DateTimeOffset | NotificationDate | | 
|  string | TranslatedNotificationType | Localized notification type name |
|  string | FormattedNotificationTypeDescription | Formatted notification type description  |
|  List of [Notification Fields](NotificationFieldViewModel2) | NotificationFields | | 

<a name="NotificationFieldViewModel2"></a>
**Notification Field**

| Type | Name | Description |
|---|---|---|
| string | Name | | 
| string | Value | |

Notifications OData
-------------------------
| **** | **** |
|---|---|
| URL | ~/odata/LogEntries |
| Available Methods | GET, POST, PUT, PATCH, DELETE |
| Parameters | OData URI parameters  | 
| Authorize | Administrator, Insured Person*  | 
| Max Expansion Depth | 2 | 

*Insured Person sees only notification addressed him / her. Administrator sees all notifications

###Notification Object

| Propety | Type | Nullable | Store Pattern | Max Length | Fixed Length | Unicode | Precision | Scale | Description |
|---|---|---|---|---|---|---|---|---|---|
|	Value	|	String	|	FALSE	|		|	400	|	FALSE	|	TRUE	|		|		|
|	Id	|	Int32	|	FALSE	|	Identity	|		|		|		|		|		|
|	NotificationType	|	Self.NotificationType	|	FALSE	|		|		|		|		|		|		|
|	DateCreated	|	DateTimeOffset	|	FALSE	|		|		|		|		|		|		|

Normally Notification object is expanded with Notification Fields

###Notification Fields Object
| Propety | Type | Nullable | Store Pattern | Max Length | Fixed Length | Unicode | Precision | Scale | Description |
|---|---|---|---|---|---|---|---|---|---|
|	Id	|	Int32	|	FALSE	|	Identity	|		|		|		|		|		|
|	NotificationId	|	Int32	|	FALSE	|		|		|		|		|		| 		| Reference to notification
|	Name	|	String	|	FALSE	|		|	50	|	FALSE	|	TRUE	|		|		|
|	Value	|	String	|	FALSE	|		|	400	|	FALSE	|	TRUE	|		|		|

Pages
-----
<span class="todo">FUTURE CHANGE: Not implemented</span>

Password - User Exists
--------
 Check if user with such username exists.

<span class="todo">FUTURE CHANGE: Find more proper name and place for this method</span>
<span class="todo">FUTURE CHANGE: Restrict access to administrator only</span>

| **** | **** |
|---|---|
| URL | ~/api/Password/UserExists |
| Method | GET |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Boolean |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| username | string | yes | Username |

<a name="Password-SendReset"></a>
Password - Send Reset
--------
Find user account and create reset invitation notification.

<span class="todo">FUTURE CHANGE: Replase word "Send" with something more appropriate</span>

<span class="todo">FUTURE CHANGE: Restrict access to administrator and target user only, make username optional if authenticated user is target user</span>

<span class="todo">FUTURE CHANGE: Replace verb to POST</span>

| **** | **** |
|---|---|
| URL | ~/api/Password/SendReset |
| Method | GET |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| username | string | yes | Username |

<a name="Password-Reset"></a>
Password - Reset
--------

<span class="todo">FUTURE CHANGE: Restrict access to administrator and target user only, make username optional if authenticated user is target user</span>

| **** | **** |
|---|---|
| URL | ~/api/Password/Reset |
| Method | POST |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Boolean |

**Request Object**

| Type | Name | Description |
|---|---|---|
|  string | ResetToken | Token generated within [Password Init Reset](Password-SendReset) | 
|  string | NewPassword | |
|  string | ConfirmPassword | |

<a name="Password-ResetWithCode"></a>
Password - Reset with code
--------

<span class="todo">FUTURE CHANGE: Remove duplication of Code and Token</span>

<span class="todo">FUTURE CHANGE: Restrict access to administrator and target user only, make username optional if authenticated user is target user</span>

| **** | **** |
|---|---|
| URL | ~/api/Password/ResetWithCode |
| Method | POST |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Boolean |

**Request Object**

| Type | Name | Description |
|---|---|---|
|  string | ResetCode | Code generated within [Password Init Reset](Password-SendReset) | 
|  string | NewPassword | |
|  string | ConfirmPassword | |

Password - Token is valid
--------
 Check if password reset token is still valid.

<span class="todo">FUTURE CHANGE: Find more proper name for this method, add verb to name</span>

| **** | **** |
|---|---|
| URL | ~/api/Password/TokenIsValid |
| Method | GET |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Boolean |


**Request Parameters**

<span class="todo">FUTURE CHANGE: Remove duplication of Code and Token or at least fix naming</span>

| Name | Type | Required | Description |
|---|---|---|---|
| code | string | yes | Token generated within [Password Init Reset](Password-SendReset)|

Password - Cancel Reset
--------
Invalidate password reset token generated within [Password Init Reset](Password-SendReset)

| **** | **** |
|---|---|
| URL | ~/api/Password/CancelReset |
| Method | GET |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Boolean: true - token was invalidated, false - token was already invalid |

<span class="todo">FUTURE CHANGE: Restrict access to administrator and target user only, make username optional if authenticated user is target user</span>

**Request Parameters**

<span class="todo">FUTURE CHANGE: Remove duplication of Code and Token or at least fix naming</span>

| Name | Type | Required | Description |
|---|---|---|---|
| code | string | yes | Token generated within [Password Init Reset](Password-SendReset)|

Places
------
Returns list of geofences defined in the system.

| **** | **** |
|---|---|
| URL | ~/api/Places |
| Method | GET |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |
| Response | Array of JSON objects |

**Response Object (Item in the array) **

| Type | Name | Description |
|---|---|---|
|  int | PlaceId | | 
|  int | ParentId | Reverence to a parent geofence | 
|  string | Name | | 
|  int | Level | Level of the geofence starting from bigger geofences to smaller ones |


Policies - Admin Stats
------------------
Returns numbers of policies of different types.

<span class="todo">FUTURE CHANGE: Make it HDI like</span>

| **** | **** |
|---|---|
| URL | ~/api/Policies/AdminStats |
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |

**Response Object **

| Type | Name | Description |
|---|---|---|
| int | NumberOfObdPolicies | |
| int | NumberOf846Policies | |
| int | NumberOfPolicies | |
| int | NumberOfObdActivatedPolicies | |
| int | NumberOf846ActivatedPolicies | |
| int | NumberOfActivatedPolicies | |
| int | NumberOfObdRegistredPolicies | |
| int | NumberOf846RegistredPolicies | |
| int | NumberOfRegistredPolicies | |
| int | NumberOfObdActivatedAndRegistredPolicies | |
| int | NumberOf846ActivatedAndRegistredPolicies | |
| int | NumberOfActivatedAndRegistredPolicies | |

Policies - Add
------------------
| **** | **** |
|---|---|
| URL | ~/api/Policies/Add |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | Number | yes | Unique policy number |    
| [DeviceType](DeviceType) | DeviceType | yes | |        
| [DeliveryMethod](DeliveryMethod) | RequestedDeviceType | yes | |
| DateTimeOffset? | SignDate | | |
| DateTimeOffset? | StartDate | | |
| DateTimeOffset? | ExpirationDate | | |
| JSON Object | Customer | | |
| string | Customer.Number | yes, if Customer container passed | Unique customer number|
| string | Customer.Name | | |      
| JSON Object | Vehicle | yes | |
| string | Vehicle.VIN | yes | |
| string  | Vehicle.HSN | | |
| string | Vehicle.TSN | | |
| string | Vehicle.MakeModelCode | yes | |
| string | Vehicle.LicensePlate | | |
| [VehicleColor](VehicleColor) | Vehicle.Color | | |
| string | Vehicle.Make | | |
| string | Vehicle.Model | | |
| int | Vehicle.YearOfInitialRegistration | yes | |
| [MotorType](MotorType) | Vehicle.MotorType | | |

<span class="todo">FUTURE CHANGE: Make use of "Custom" parameter</span>

**Response Object **

| Type | Name | Description |
|---|---|---|
| int | Id | Policy ID |        
| int PolicyProviderId | Must be ignored by client |
| int | CustomerId | Nullable, not null if customer was passed |
| string | Number | Policy number |
| int | DeliveryPersonId | Nullable, always null |
| [DeviceType](DeviceType) | DeviceType | |        
| DateTimeOffset | SignDate | Nullable |
| DateTimeOffset | StartDate | |
| ateTimeOffset | ExpirationDate | Nullable |
| DateTimeOffset | TerminationDate | Nullable |
| string | UniqueRequest | Unique, system internal policy code|
| DateTimeOffset | SecurityAccepted | Nullable, always null |
| [PolicyStatus](PolicyStatus) | Status | |
| ContactPerson DeliveryPerson | PolicyStatus  |
| JSON Object | Customer | presents only if was passed |
| int | Customer.Id | Unique, system internal Customer ID |
| int | Customer.Number | Customer Number |
| int | Customer.Name | Nullable |
| int | Customer.ContactPersonId | Must be ignored by client |
| int | Customer.ContactPerson | Must be ignored by client |
| JSON Object | PolicyProvider | Must be ignored by client |
| JSON Array | CustomPolicyFields | Must be ignored by client |
| JSON Array of [PolicyVehicleUnit](PolicyVehicleUnit1)  | PolicyVehicleUnits | Must be ignored by client |

<a name="PolicyVehicleUnit1"></a>
**PolicyVehicleUnit Object**

| Type | Name | Description |
|---|---|---|
| bool | IsActivated | Unit is activated |
| int | Id | PolicyVehicleUnit ID |
| int | PolicyId |  |
| int | UnitId |  Nullable |
| int | VehicleId | |
| bool| AllowMultipleRegistrations | Must be ignored by client, always null |
| short | UnitInstallLocation | Must be ignored by client, always null |
| DateTimeOffset | UnitAssignmentDate | Nullable  |
| DateTimeOffset | UnitActivationDate | Nullable  |
| string | TrackingNumber | Delivery tracking number |
| bool | IsExpired | Unit was deactivated |
| DateTimeOffset | TerminationDate |  |
| Guid | MZoneVehicleId | Must be ignored by client, nullable |
| int | PlaceId | Nullable, must be ignored by client |
| JSON Array | Accidents | Must be ignored by client, always null |
| JSON Array | Drivers | Must be ignored by client, always null |
| JSON Object | Unit | Must be ignored by client, always null |
| JSON Object | Vehicle | |
| string | Vehicle.VIN | |
| string  | Vehicle.HSN | |
| string | Vehicle.TSN | |
| string | Vehicle.MakeModelCode | |
| string | Vehicle.LicensePlate | |
| [VehicleColor](VehicleColor) | Vehicle.Color | |
| string | Vehicle.Make | |
| string | Vehicle.Model | |
| int | Vehicle.YearOfInitialRegistration | |
| [MotorType](MotorType) | Vehicle.MotorType | |
| JSON Object | Place | Must be ignored by client |
| JSON Array |PolicyVehicleUnitUsers | Must be ignored by client |
| JSON Array |Shares | Must be ignored by client |
| JSON Array |UnitReplacements | Must be ignored by client |

Policies - Renew
------------------
| **** | **** |
|---|---|
| URL | ~/api/Policies/Renew |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object with created policy data |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| policyNumber | string | yes | policy to renew |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | NewNumber| yes | New unique policy number |    
| DateTimeOffset | RenewDate| no | Date of renewal, current date is used if not passed |   

**Response Object **

| Type | Name | Description |
|---|---|---|
| int | Id | Policy ID |        
| int PolicyProviderId | Must be ignored by client |
| int | CustomerId | Nullable, not null if customer was passed |
| string | Number | Policy number |
| int | DeliveryPersonId | Nullable, always null |
| [DeviceType](DeviceType) | DeviceType | |        
| DateTimeOffset | SignDate | Nullable |
| DateTimeOffset | StartDate | |
| ateTimeOffset | ExpirationDate | Nullable |
| DateTimeOffset | TerminationDate | Nullable |
| string | UniqueRequest | Unique, system internal policy code|
| DateTimeOffset | SecurityAccepted | Nullable, always null |
| [PolicyStatus](PolicyStatus) | Status | |
| ContactPerson DeliveryPerson | PolicyStatus  |
| JSON Object | Customer | presents only if was passed |
| int | Customer.Id | Unique, system internal Customer ID |
| int | Customer.Number | Customer Number |
| int | Customer.Name | Nullable |
| int | Customer.ContactPersonId | Must be ignored by client |
| int | Customer.ContactPerson | Must be ignored by client |
| JSON Object | PolicyProvider | Must be ignored by client |
| JSON Array | CustomPolicyFields | Must be ignored by client |
| JSON Array of [PolicyVehicleUnit](PolicyVehicleUnit2)  | PolicyVehicleUnits | Must be ignored by client |

<a name="PolicyVehicleUnit2"></a>
**PolicyVehicleUnit Object**

| Type | Name | Description |
|---|---|---|
| bool | IsActivated | Unit is activated |
| int | Id | PolicyVehicleUnit ID |
| int | PolicyId |  |
| int | UnitId |  Nullable |
| int | VehicleId | |
| bool| AllowMultipleRegistrations | Must be ignored by client, always null |
| short | UnitInstallLocation | Must be ignored by client, always null |
| DateTimeOffset | UnitAssignmentDate | Nullable  |
| DateTimeOffset | UnitActivationDate | Nullable  |
| string | TrackingNumber | Delivery tracking number |
| bool | IsExpired | Unit was deactivated |
| DateTimeOffset | TerminationDate |  |
| Guid | MZoneVehicleId | Must be ignored by client, nullable |
| int | PlaceId | Nullable, must be ignored by client |
| JSON Array | Accidents | Must be ignored by client, always null |
| JSON Array | Drivers | Must be ignored by client, always null |
| JSON Object | Unit | Linked Unit |
|int | Unit.Id | |
|string | Unit.SerialNumber | |
| JSON Object | Vehicle | |
| string | Vehicle.VIN | |
| string  | Vehicle.HSN | |
| string | Vehicle.TSN | |
| string | Vehicle.MakeModelCode | |
| string | Vehicle.LicensePlate | |
| [VehicleColor](VehicleColor) | Vehicle.Color | |
| string | Vehicle.Make | |
| string | Vehicle.Model | |
| int | Vehicle.YearOfInitialRegistration | |
| [MotorType](MotorType) | Vehicle.MotorType | |
| JSON Object | Place | Must be ignored by client |
| JSON Array |PolicyVehicleUnitUsers | Must be ignored by client |
| JSON Array |Shares | Must be ignored by client |
| JSON Array |UnitReplacements | Must be ignored by client |

Policies - Renew
------------------
| **** | **** |
|---|---|
| URL | ~/api/Policies/Renew |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object with created policy data |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| policyNumber | string | yes | policy to renew |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | NewNumber| yes | New unique policy number |    
| DateTimeOffset | RenewDate| no | Date of renewal, current date is used if not passed |   

**Response Object **

| Type | Name | Description |
|---|---|---|
| int | Id | Policy ID |        
| int PolicyProviderId | Must be ignored by client |
| int | CustomerId | Nullable, not null if customer was passed |
| string | Number | Policy number |
| int | DeliveryPersonId | Nullable, always null |
| [DeviceType](DeviceType) | DeviceType | |        
| DateTimeOffset | SignDate | Nullable |
| DateTimeOffset | StartDate | |
| ateTimeOffset | ExpirationDate | Nullable |
| DateTimeOffset | TerminationDate | Nullable |
| string | UniqueRequest | Unique, system internal policy code|
| DateTimeOffset | SecurityAccepted | Nullable, always null |
| [PolicyStatus](PolicyStatus) | Status | |
| ContactPerson DeliveryPerson | PolicyStatus  |
| JSON Object | Customer | presents only if was passed |
| int | Customer.Id | Unique, system internal Customer ID |
| int | Customer.Number | Customer Number |
| int | Customer.Name | Nullable |
| int | Customer.ContactPersonId | Must be ignored by client |
| int | Customer.ContactPerson | Must be ignored by client |
| JSON Object | PolicyProvider | Must be ignored by client |
| JSON Array | CustomPolicyFields | Must be ignored by client |
| JSON Array of [PolicyVehicleUnit](PolicyVehicleUnit2)  | PolicyVehicleUnits | Must be ignored by client |

<a name="PolicyVehicleUnit2"></a>
**PolicyVehicleUnit Object**

| Type | Name | Description |
|---|---|---|
| bool | IsActivated | Unit is activated |
| int | Id | PolicyVehicleUnit ID |
| int | PolicyId |  |
| int | UnitId |  Nullable |
| int | VehicleId | |
| bool| AllowMultipleRegistrations | Must be ignored by client, always null |
| short | UnitInstallLocation | Must be ignored by client, always null |
| DateTimeOffset | UnitAssignmentDate | Nullable  |
| DateTimeOffset | UnitActivationDate | Nullable  |
| string | TrackingNumber | Delivery tracking number |
| bool | IsExpired | Unit was deactivated |
| DateTimeOffset | TerminationDate |  |
| Guid | MZoneVehicleId | Must be ignored by client, nullable |
| int | PlaceId | Nullable, must be ignored by client |
| JSON Array | Accidents | Must be ignored by client, always null |
| JSON Array | Drivers | Must be ignored by client, always null |
| JSON Object | Unit | Linked Unit |
|int | Unit.Id | |
|string | Unit.SerialNumber | |
| JSON Object | Vehicle | |
| string | Vehicle.VIN | |
| string  | Vehicle.HSN | |
| string | Vehicle.TSN | |
| string | Vehicle.MakeModelCode | |
| string | Vehicle.LicensePlate | |
| [VehicleColor](VehicleColor) | Vehicle.Color | |
| string | Vehicle.Make | |
| string | Vehicle.Model | |
| int | Vehicle.YearOfInitialRegistration | |
| [MotorType](MotorType) | Vehicle.MotorType | |
| JSON Object | Place | Must be ignored by client |
| JSON Array |PolicyVehicleUnitUsers | Must be ignored by client |
| JSON Array |Shares | Must be ignored by client |
| JSON Array |UnitReplacements | Must be ignored by client |

Policies - Transfer 
------------------
| **** | **** |
|---|---|
| Description | Transfer Unit to new Vehicle and Policy |
| URL | ~/api/Policies/Transfer |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object with created policy data |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| policyNumber | string | yes | policy to transfer unit from |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| JSON Object |  RenewData | yes | | 
| string | RenewData.NewNumber| yes | New unique policy number |    
| DateTimeOffset | RenewData.RenewDate| no | Date of renewal, current date is used if not passed |    
| JSON Object | Vehicle | yes  |  | 
| string | Vehicle.VIN | yes | |
| string  | Vehicle.HSN | | |
| string | Vehicle.TSN | | |
| string | Vehicle.MakeModelCode | yes | |
| string | Vehicle.LicensePlate | | |
| [VehicleColor](VehicleColor) | Vehicle.Color | | |
| string | Vehicle.Make | | |
| string | Vehicle.Model | | |
| int | Vehicle.YearOfInitialRegistration | yes | |
| [MotorType](MotorType) | Vehicle.MotorType | | |  

**Response Object **

| Type | Name | Description |
|---|---|---|
| int | Id | Policy ID |        
| int PolicyProviderId | Must be ignored by client |
| int | CustomerId | Nullable, not null if customer was passed |
| string | Number | Policy number |
| int | DeliveryPersonId | Nullable, always null |
| [DeviceType](DeviceType) | DeviceType | |        
| DateTimeOffset | SignDate | Nullable |
| DateTimeOffset | StartDate | |
| ateTimeOffset | ExpirationDate | Nullable |
| DateTimeOffset | TerminationDate | Nullable |
| string | UniqueRequest | Unique, system internal policy code|
| DateTimeOffset | SecurityAccepted | Nullable, always null |
| [PolicyStatus](PolicyStatus) | Status | |
| ContactPerson DeliveryPerson | PolicyStatus  |
| JSON Object | Customer | presents only if was passed |
| int | Customer.Id | Unique, system internal Customer ID |
| int | Customer.Number | Customer Number |
| int | Customer.Name | Nullable |
| int | Customer.ContactPersonId | Must be ignored by client |
| int | Customer.ContactPerson | Must be ignored by client |
| JSON Object | PolicyProvider | Must be ignored by client |
| JSON Array | CustomPolicyFields | Must be ignored by client |
| JSON Array of [PolicyVehicleUnit](PolicyVehicleUnit3)  | PolicyVehicleUnits | Must be ignored by client |

<a name="PolicyVehicleUnit3"></a>
**PolicyVehicleUnit Object**

| Type | Name | Description |
|---|---|---|
| bool | IsActivated | Unit is activated |
| int | Id | PolicyVehicleUnit ID |
| int | PolicyId |  |
| int | UnitId |  Nullable |
| int | VehicleId | |
| bool| AllowMultipleRegistrations | Must be ignored by client, always null |
| short | UnitInstallLocation | Must be ignored by client, always null |
| DateTimeOffset | UnitAssignmentDate | Nullable  |
| DateTimeOffset | UnitActivationDate | Nullable  |
| string | TrackingNumber | Delivery tracking number |
| bool | IsExpired | Unit was deactivated |
| DateTimeOffset | TerminationDate |  |
| Guid | MZoneVehicleId | Must be ignored by client, nullable |
| int | PlaceId | Nullable, must be ignored by client |
| JSON Array | Accidents | Must be ignored by client, always null |
| JSON Array | Drivers | Must be ignored by client, always null |
| JSON Object | Unit | Linked Unit |
|int | Unit.Id | |
|string | Unit.SerialNumber | |
| JSON Object | Vehicle | |
| string | Vehicle.VIN | |
| string  | Vehicle.HSN | |
| string | Vehicle.TSN | |
| string | Vehicle.MakeModelCode | |
| string | Vehicle.LicensePlate | |
| [VehicleColor](VehicleColor) | Vehicle.Color | |
| string | Vehicle.Make | |
| string | Vehicle.Model | |
| int | Vehicle.YearOfInitialRegistration | |
| [MotorType](MotorType) | Vehicle.MotorType | |
| JSON Object | Place | Must be ignored by client |
| JSON Array |PolicyVehicleUnitUsers | Must be ignored by client |
| JSON Array |Shares | Must be ignored by client |
| JSON Array |UnitReplacements | Must be ignored by client |

Policies - Replace
------------------
| **** | **** |
|---|---|
| Description | Replace Unit for Policy |
| URL | ~/api/Policies/Replace |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object with created policy data |


**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | PolicyNumber| yes | Policy number |    
| [DeviceType](DeviceType) | DeviceType | yes | |        
| [DeliveryMethod](DeliveryMethod) | DeliveryMethod | yes | |


<span class="todo">FUTURE CHANGE: Treat policy number in a same way for all Policy controller's Requests</span> 

**Response Object **

<span class="todo">FUTURE CHANGE: Filter response for all Policy controller's Requests</span> 

| Type | Name | Description |
|---|---|---|
| int | Id | Policy ID |        
| int PolicyProviderId | Must be ignored by client |
| int | CustomerId | Nullable, not null if customer was passed |
| string | Number | Policy number |
| int | DeliveryPersonId | Nullable, always null |
| [DeviceType](DeviceType) | DeviceType | |        
| DateTimeOffset | SignDate | Nullable |
| DateTimeOffset | StartDate | |
| ateTimeOffset | ExpirationDate | Nullable |
| DateTimeOffset | TerminationDate | Nullable |
| string | UniqueRequest | Unique, system internal policy code|
| DateTimeOffset | SecurityAccepted | Nullable, always null |
| [PolicyStatus](PolicyStatus) | Status | |
| ContactPerson DeliveryPerson | PolicyStatus  |
| JSON Object | Customer | presents only if was passed |
| int | Customer.Id | Unique, system internal Customer ID |
| int | Customer.Number | Customer Number |
| int | Customer.Name | Nullable |
| int | Customer.ContactPersonId | Must be ignored by client |
| int | Customer.ContactPerson | Must be ignored by client |
| JSON Object | PolicyProvider | Must be ignored by client |
| JSON Array | CustomPolicyFields | Must be ignored by client |
| JSON Array of [PolicyVehicleUnit](PolicyVehicleUnit4)  | PolicyVehicleUnits | Must be ignored by client |

<a name="PolicyVehicleUnit4"></a>
**PolicyVehicleUnit Object**

| Type | Name | Description |
|---|---|---|
| bool | IsActivated | Unit is activated |
| int | Id | PolicyVehicleUnit ID |
| int | PolicyId |  |
| int | UnitId |  Nullable |
| int | VehicleId | |
| bool| AllowMultipleRegistrations | Must be ignored by client, always null |
| short | UnitInstallLocation | Must be ignored by client, always null |
| DateTimeOffset | UnitAssignmentDate | Nullable  |
| DateTimeOffset | UnitActivationDate | Nullable  |
| string | TrackingNumber | Delivery tracking number |
| bool | IsExpired | Unit was deactivated |
| DateTimeOffset | TerminationDate |  |
| Guid | MZoneVehicleId | Must be ignored by client, nullable |
| int | PlaceId | Nullable, must be ignored by client |
| JSON Array | Accidents | Must be ignored by client, always null |
| JSON Array | Drivers | Must be ignored by client, always null |
| JSON Object | Unit | Linked Unit |
|int | Unit.Id | |
|string | Unit.SerialNumber | |
| JSON Object | Vehicle | |
| string | Vehicle.VIN | |
| string  | Vehicle.HSN | |
| string | Vehicle.TSN | |
| string | Vehicle.MakeModelCode | |
| string | Vehicle.LicensePlate | |
| [VehicleColor](VehicleColor) | Vehicle.Color | |
| string | Vehicle.Make | |
| string | Vehicle.Model | |
| int | Vehicle.YearOfInitialRegistration | |
| [MotorType](MotorType) | Vehicle.MotorType | |
| JSON Object | Place | Must be ignored by client |
| JSON Array |PolicyVehicleUnitUsers | Must be ignored by client |
| JSON Array |Shares | Must be ignored by client |
| JSON Array |UnitReplacements | Must be ignored by client |

Policies - Terminate
------------------
| **** | **** |
|---|---|
| Description | Terminate policy |
| URL | ~/api/Policies/Terminate |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| policyNumber | string | yes | policy to transfer unit from |

Policies OData
--------------
| **** | **** |
|---|---|
| URL | ~/odata/LogEntries |
| Available Methods | GET, PUT, POST, PATCH, DELETE |
| Parameters | OData URI parameters  | 
| Authorize | Administrator  | 
| Max Expansion Depth | 2 | 

###Policy Object

| Propety | Type | Nullable | Store Pattern | Max Length | Fixed Length | Unicode | Precision | Scale | Description |
|---|---|---|---|---|---|---|---|---|---|
|	Id	|	Int32	|	FALSE	|	Identity	|		|		|		|		|		|
|	PolicyProviderId	|	Int32	|	FALSE	|		|		|		|		|		|		|
|	CustomerId	|	Int32	|		|		|		|		|		|		|		|
|	Number	|	String	|	FALSE	|		|	50	|	FALSE	|	TRUE	|		|		|
|	DeliveryPersonId	|	Int32	|		|		|		|		|		|		|		|
|	DeviceType	|	Self.DeviceType	|	FALSE	|		|		|		|		|		|		|
|	SignDate	|	DateTimeOffset	|		|		|		|		|		|		|		|
|	StartDate	|	DateTimeOffset	|	FALSE	|		|		|		|		|		|		|
|	ExpirationDate	|	DateTimeOffset	|		|		|		|		|		|		|		|
|	TerminationDate	|	DateTimeOffset	|		|		|		|		|		|		|		|
|	UniqueRequest	|	String	|	FALSE	|		|	50	|	FALSE	|	TRUE	|		|		|
|	SecurityAccepted	|	DateTimeOffset	|		|		|		|		|		|		|		|
|	Status	|	Self.PolicyStatus	|	FALSE	|		|		|		|		|		|		|



PolicyVehicleUnits
------------------

...

PolicyVehicleUnits OData
------------------

...

Reports
-------

...

Shares
------

...

Translations
------------

...

Trips
-----

...

Users OData
-----------

...

VehicleMakeModels - Post
------------------
| **** | **** |
|---|---|
| Description | Report new Make-Model code |
| URL | ~/api/VehicleMakeModels |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | 204	No Content |


**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | DataCode | yes | New Make-Model code  |    
| int | YearIntroduced | yes | Vehicle introduction year |    
| int | YearDiscontinued | yes | Year of the end of vehicle manufactoring | 
| string | MakeDescription | yes | Vehicle make | 
| string | ModelDescription | yes | Vehicle model | 


VehicleMakeModels - Get
------------------
Get OBD port location by Make-Model code


| **** | **** |
|---|---|
| URL | ~/api/VehicleMakeModel |
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |

| Parameters |  |
|---|---|
| dataCode | Make-Model code  |


**Response Object **

| Type | Name | Description |
|---|---|---|
| int | ObdPortLocationId | instalation location ID |
| string | DataCode | New Make-Model code |
| int | YearIntroduced | Vehicle introduction year |
| int | YearDiscontinued | Year of the end of vehicle manufactoring  |
| string | MakeDescription | Vehicle make |
| string | ModelDescription |Vehicle model |






Vehicles
-----------------

...

CONFIDENTIAL
Copyright 2002 by Scope Logistical Solutions (Pty) Ltd

All rights reserved. No part of this publication may be copied, reproduced or transmitted in any formor by anymeans, electronic, mechanical, photocopying, recording, or otherwise, save with written permission from Scope Technologies or in accordance with the provisions of Copyright Act no. 98 of 1978 (as amended).
