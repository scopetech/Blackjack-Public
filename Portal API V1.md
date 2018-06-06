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

### Request

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

### Response

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
All classifiers in the system is declared inside **Blackjack.Shared.Classifiers** namespace, use it for OData requests.  
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
        Custom = 7,
        LastDays = 8
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
        Unknown = 0,
        MHubObd = 1, // connects to OBD port
        MHubBattery = 2, // connects to Battery
        Soft = 3, // Software typically installed on Mobile phone, acting as an MHub
        ThirdParty = 4,
        Virtual = 5 // Trips recorded on multiple devices
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
        FirstTripCompletion = 9, //MLog
        PluggedOut = 10, //MLog
        PluggedIn = 11, //MLog
        DifferentVehicleIdentified = 12, //MLog
        NeverReported = 13,
        NotReporting = 14, //MLog
        NoImAlive = 15,
        DeviceToBeReturned = 16, //MLog
        DeviceReturned = 17, //MLog
        ShareInvitation = 18,
        PolicyRenewal = 19,
        UnitTransfer = 20,
        OutOfArea = 21,
        FirstTripInvitation = 24,
        ShareAccepted = 25,
        RegularServiceDue = 26,
        DiagnosticTroubleCodeReceived = 27,
        ServiceBooked = 28,
        BookingRescheduled = 29,
        BookingCanceled = 30,
        BookingReminder = 31,
        GroupMessage = 32,
        TrialFinished = 33,
        TestEmail = 34,
        TestPushNotification = 35,
        StageTwoUnitReceived = 36,
        DeviceStoppedReporting = 37, //MLog
	TokenEmail = 38,
	GamificationSummaryForPeriod = 39,
        RuleInstanceNotification = 40,
        RuleInstanceUpdateCommandNotification = 41,
        HeritageWelcome = 42,
        HeritageMidTrial = 43,
        HeritageEndTrial = 44,
    }    

<a name="MLogNotificationMapping">MLog Notification Mapping</a>

                case NotificationTemplateType.FirstTripEndEvent -> NotificationType.FirstTripCompletion;
                case NotificationTemplateType.UnitPlugoutEvent -> NotificationType.PluggedOut;
                case NotificationTemplateType.UnitPluginEvent -> NotificationType.PluggedIn;
                case NotificationTemplateType.VehicleSignatureEvent -> NotificationType.DifferentVehicleIdentified;
                case NotificationTemplateType.UnitDispatchedNoTripEnd -> NotificationType.NotReporting;
                case NotificationTemplateType.UnitUninstalledNotReceivedNotificationTypeNotificationType  -> otificationType.DeviceToBeReturned;
                case NotificationTemplateType.UnitReceivedFromCustomer -> NotificationTypeNotificationType.DeviceReturned;
                case NotificationTemplateType.UnitStoppedReporting -> NotificationType.DeviceStoppedReporting;



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

<a name="RuleType"></a>  
  
    /// <summary>
    /// Types of rule for Location-based services
    /// </summary>
     public enum RuleType
    {
        PlaceEntry = 1,
        PlaceExit = 2,
        DoNotEnterPlace = 3,
        DoNotExitPlace = 4, // aka "Must be in given time" in Mobile UI
        MustLeaveByTimeRange = 5, // aka "Must leave in given time" in Mobile UI
        ArriveInGivenTime = 6 // aka "Reach another POI in given time" in Mobile UI
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

### MotorTypes

| **** | **** |
|---|---|
| Description | Returns MotorTypes |
| URL | ~/api/Classifiers/MotorTypes|
| Method | GET |
| Authorize | Administrator, User |

### Colors

| **** | **** |
|---|---|
| Description | Returns vehicle colors|
| URL | ~/api/Classifiers/Colors|
| Method | GET |
| Authorize | Administrator, User |

### NotificationTypes

| **** | **** |
|---|---|
| Description | Returns Notification Types|
| URL | ~/api/Classifiers/NotificationTypes|
| Method | GET |
| Authorize | Administrator, User |

### DistanceUnits

| **** | **** |
|---|---|
| Description | Returns distance units|
| URL | ~/api/Classifiers/DistanceUnits|
| Method | GET |
| Authorize | Administrator, User |

### ServiceJobTypes

| **** | **** |
|---|---|
| Description | Returns vehicle service & maintenance job types |
| URL | ~/api/Classifiers/ServiceJobTypes |
| Method | GET |
| Authorize | Administrator, User |

### ServiceJobStatuses

| **** | **** |
|---|---|
| Description | Returns vehicle service & maintenance job statuses |
| URL | ~/api/Classifiers/ServiceJobStatuses |
| Method | GET |
| Authorize | Administrator, User |

Account - Change Password
-------

| **** | **** |
|---|---|
| URL | ~/api/Account/ChangePassword |
| Method | Post |
| Authorize | Administrator, User  | 

### Request Payload Sample:

    {
    "OldPassword":"0!dP@ssw0rd", //required
    "newPassword":"NewP@ssw0rd", //required
    "confirmPassword":"NewP@ssw0rd" //required, must be same as "newPassword"
    }

### Response 
Status Code ( 200 OK or 500 Internal Server Error)

Account - Change UserName
-------

| **** | **** |
|---|---|
| URL | ~/api/Account/{oldName}/ChangePassword |
| Method | Post |
| Authorize | Administrator, User, Insured Person  | 

### Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| newName | string | yes | |

### Response 
Status Code ( 204 NoContent or 404 NotFound)

Account - Register with Policy
-------

| **** | **** |
|---|---|
| Description | Register Insured Person with customer/policy number and unit number|
| URL | ~/api/Account/RegisterWithPolicy |
| Method | POST |
| Authorize | Anonymous | 
    
### Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| UserName | string | yes | |
| Password | string | yes | |
| PolicyOrCustomerNumber | string | yes | |
| SerialNumber| string | yes | |
| AgreementAccepted| bool | yes | |  

### Response 
Integer - PolicyVehicleUnit ID 

Account - Register with Share
-------
| **** | **** |
|---|---|
| Description | Register Insured Person with share code |
| URL | ~/api/Account/RegisterWithShare |
| Method | POST |
| Authorize | Anonymous | 

### Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| SharedCode| string | yes | |
| UserName | string | yes | |
| Password | string | yes | |
| AgreementAccepted| bool | yes | | 

### Response 
Integer - PolicyVehicleUnit ID 

Account - Register with Email
-------
| **** | **** |
|---|---|
| Description | Register Insured Person with email |
| URL | ~/api/Account/RegisterWithEmail |
| Method | POST |
| Authorize | Administrator | 

### Request Payload Object Properties   

| Property | Type| Required| Description |
|---|---|---|---|
| Email | string | yes | |
| PolicyNumber | string | yes | |

 
Get Accidents Reports in PDF 
---------
  
### Request

| **** | **** |
|---|---|
| URL | ~/api/Accidents/ReportInPdf |
| Method | GET |

| Parameters |  |
|---|---|
| id | ID of accident  |

### Response

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

### Accident Object

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
### Policy
<span class="todo">FUTURE CHANGE: Pluralize name</span>

| **** | **** |
|---|---|
| Description | Get CSV file containing all Policies |
| URL | ~/api/Csv/Policy|
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | text/csv |

### Trip
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

### Accident
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

### Request Payload Object Properties   

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

### Response 
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

### Response   

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
### Insured Vehicle Object

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
|  int | DrivingTime | seconds |
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
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | PolicyVehicleUnit ID |
| beginDate | DateTimeOffset| yes  | Filter date start |
| endDate| DateTimeOffset | yes |  Filter date end |

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

InsuredPersons - EventsForDateRange
--------------
| **** | **** |
|---|---|
| Description | Get paged data of the vehicle events filtered by date range |
| URL | ~/api/InsuredPersons/EventsForDateRange |
| Method | GET |
| Authorize | Administrator, Insured Person |
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

### Log Entry Object

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
| URL | ~/odata/Notifications |
| Available Methods | GET, POST, PUT, PATCH, DELETE |
| Parameters | OData URI parameters  | 
| Authorize | Administrator, Insured Person*  | 
| Max Expansion Depth | 2 | 

*Insured Person sees only notification addressed him / her. Administrator sees all notifications

### Notification Object

| Propety | Type | Nullable | Store Pattern | Max Length | Fixed Length | Unicode | Precision | Scale | Description |
|---|---|---|---|---|---|---|---|---|---|
|	Value	|	String	|	FALSE	|		|	400	|	FALSE	|	TRUE	|		|		|
|	Id	|	Int32	|	FALSE	|	Identity	|		|		|		|		|		|
|	NotificationType	|	Self.NotificationType	|	FALSE	|		|		|		|		|		|		|
|	DateCreated	|	DateTimeOffset	|	FALSE	|		|		|		|		|		|		|

Normally Notification object is expanded with Notification Fields

### Notification Fields Object
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

**Response Object (Item in the array)**

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

**Response Object**

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

Subsctiption - Add Policy With User Registration
------------------
| **** | **** |
|---|---|
| URL | ~/api/Subscriptions/PolicyAddWithUserRegistration |
| Method | POST |
| Authorize | Anonymous |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| JSON Object | Policy | yes | |    
| string | Policy.Number | yes | Unique policy number |    
| [DeviceType](DeviceType) | Policy.DeviceType | yes | |        
| [DeliveryMethod](DeliveryMethod) | Policy.RequestedDeviceType | yes | |
| DateTimeOffset? | Policy.SignDate | | |
| DateTimeOffset? | Policy.StartDate | | |
| DateTimeOffset? | Policy.ExpirationDate | | |
| JSON Object | Policy.Customer | | |
| string | Policy.Customer.Number | yes, if Customer container passed | Unique customer number|
| string | Policy.Customer.Name | | |      
| JSON Object | Policy.Customer.Contacts | | |  
| string | Policy.Customer.Contacts.FirstName | | |  
| string | Policy.Customer.Contacts.Name | | |  
| string | Policy.Customer.Contacts.Title | | |  
| string | Policy.Customer.Contacts.Email | | |  
| [PersonType](PersonType) | Policy.Customer.Contacts.Type | | |  
| string | Policy.Customer.Contacts.ZipCode | | |  
| string | Policy.Customer.Contacts.Country | | |  
| string | Policy.Customer.Contacts.City | | |  
| string | Policy.Customer.Contacts.Address | | |  
| string | Policy.Customer.Contacts.CompanyName | | |  
| string | Policy.Customer.Contacts.Phone | | |  
| string | Policy.Customer.Contacts.MobilePhone | | |  
| JSON Object | Policy.Vehicle | yes | |
| string | Policy.Vehicle.VIN | yes | |
| string  | Policy.Vehicle.HSN | | |
| string | Policy.Vehicle.TSN | | |
| string | Policy.Vehicle.MakeModelCode | yes | |
| string | Policy.Vehicle.LicensePlate | | |
| [VehicleColor](VehicleColor) | Policy.Vehicle.Color | | |
| string | Policy.Vehicle.Make | | |
| string | Policy.Vehicle.Model | | |
| int | Policy.Vehicle.YearOfInitialRegistration | yes | |
| [MotorType](MotorType) | Policy.Vehicle.MotorType | | |
| JSON Object | User | yes | |    
| string | User.UserName | yes | |
| string | User.Password | yes | |
| bool | User.AgreementAccepted | yes | |
| JSON Object | User.ContactPerson | | |
| string | User.ContactPerson.FirstName | | |
| string | User.ContactPerson.Name | | |
| string | User.ContactPerson.Title | | |
| string | User.ContactPerson.Email | | |
| [PersonType](PersonType) | User.ContactPerson.Type | | |
| string | User.ContactPerson.ZipCode | | |
| string | User.ContactPerson.Country | | |
| string | User.ContactPerson.City | | |
| string | User.ContactPerson.Address | | |
| string | User.ContactPerson.CompanyName | | |
| string | User.ContactPerson.Phone | | |
| string | User.ContactPerson.MobilePhone | | |
| string | User.ContactPerson.ReceiveEmails | | |
| string | DeviceNumber | yes | |
| string | DeliveryNumber | | |
| bool | Receipt | | |


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

**Response Object**

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

Policies - ChangeExpiryDate
------------------
| **** | **** |
|---|---|
| URL | ~/api/Policies/ChangeExpiryDate |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP status |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| number | string | yes | Policy for which expiry date is modified |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| DateTimeOffset | Date | yes | Policy expiration date |   

### Response 
Status Code ( 200 OK or 400 Bad Request)

Policies - ChangeStartDate
------------------
| **** | **** |
|---|---|
| URL | ~/api/Policies/ChangeStartDate |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP status |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| number | string | yes | policy for which start date is modified |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| DateTimeOffset | Date | yes | Policy start date |   

### Response 
Status Code ( 200 OK or 400 Bad Request)


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

**Response Object**

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

Policies - Reset
------------------
| **** | **** |
|---|---|
| URL | ~/api/Policies/Reset/{number} |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP status |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| number | string | yes | Policy for which PVU will be reset |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| DateTimeOffset | Date | yes | Policy related PVU reset date |   

### Response 
Status Code ( 200 OK or 400 Bad Request)

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

**Response Object**

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

**Response Object**

Empty

<span class="todo">FUTURE CHANGE: Filter response for all Policy controller's Requests</span> 

Policies - ChangeNumber
------------------
| **** | **** |
|---|---|
| Description | Change policy number |
| URL | ~/api/Policies/ChangeNumber |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |


**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | CurrentNumber| yes | Current policy number |    
| string | NewNumber | yes | New policy number |        
| bool | ChangeOnlyInPortal | false | Change only in Portal (Default: false) |

**Response Object**

Empty

Policies - ChangeCustomerNumber
------------------
| **** | **** |
|---|---|
| Description | Change customer number |
| URL | ~/api/Policies/ChangeCustomerNumber |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |


**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | CurrentNumber| yes | Current customer number |    
| string | NewNumber | yes | New customer number |        
| bool | ChangeOnlyInPortal | false | Change only in Portal (Default: false) |

**Response Object**

Empty

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


Policies - Revert Termination
------------------

| **** | **** |
|---|---|
| Description | Revert policy to state before termination. Available after termination only and next Portal sync with TOMS. Unit must still reside with the customer. |
|TOMS Restrictions|1.	Last job must be de-installation</br>2.	Deinstallation must be successful</br>3.	Policy must be previously linked to a unit</br>4.	Policy must not be linked to a unit</br>5.	Unit must be in CRR or PLCH location</br>6.	Unit must not be receipted or dispatched after deinstallation</br>7.	Deinstallation job must be imported after Revert method deployment|
|Tests|1.	Add policy API call in Portal</br>2.	Dispatch in TOMS</br>3.	Terminate API call in Portal</br>4.	Try renew API call in Portal – it will fail!</br>5.	Revert termination (NEW!) API call in Portal</br>6.	Try renew API call in Portal – it will now succeed!|
| URL | ~/api/Policies/Terminate/Revert |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| policyNumber | string | yes | Terminated policy |


Policies - Amend
------------------

| **** | **** |
|---|---|
| Description | Update policy details after policy is added or being replaced. Available before dispatch in TOMS only. It is possible to change required device type or delivery method or both. |
|TOMS Restrictions|1.	Last job must be installation or replacement</br>2.	Last job must not be added to dispatch</br>3.	Policy must not be linked to a unit|
|Tests|1.	Add policy API call in Portal with OBD device type</br>2.	Try dispatch in TOMS with MHub 846 device type - it will fail!</br>3.	Amend API call in Portal with Mhub 846 device type</br>4.	Try dispatch in TOMS with MHub 846 device type - it will now succeed!|
| URL | ~/api/Policies/Amend |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| policyNumber | string | yes | Policy to update |
| deviceType | [DeviceType](#classifiers-enumerations) | no | New required device type |
| deliveryMethod | [DeliveryMethod](#classifiers-enumerations) | no | New delivery method |


Policies OData
--------------
| **** | **** |
|---|---|
| URL | ~/odata/Policies |
| Available Methods | GET, PUT, POST, PATCH, DELETE |
| Parameters | OData URI parameters  | 
| Authorize | Administrator  | 
| Max Expansion Depth | 2 | 

### Policy Object

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



PolicyVehicleUnits - RevokeAccess
------------------
| **** | **** |
|---|---|
| Description | Revokes access to vehicle for specific user |
| URL | ~/api/PolicyVehicleUnits/RevokeAccess |
| Method | POST |
| Authorize | Administrator, InsuredRole |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | Subscription to remove |
| userName | string | yes | User whose access should be revoked |



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

| **** | **** |
|---|---|
| Description | Get list of trips of an insured person |
| URL | ~/api/Trips |
| Method | GET |
| Authorize | Administrator, Insured Person |
| Response Content-Type | application/json; charset=utf-8 |

**Request Parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| pvu | int | yes | PolicyVehicleUnit ID |
| beginDate | DateTimeOffset| yes  | Filter date start |
| endDate| DateTimeOffset | yes |  Filter date end |
| route | bool | no |  Include vehicle positions |
| pageIndex | int | yes | Page index for paging |
| pageSize | int | yes |  Page size for paging |

**Response Object**

| Type | Name | Description |
|---|---|---|
|  int | Count | Total count of trips |
|  bool | HasMoreResults |  Represents if page is last or not |
|  List of [Trip Descriptions](#TripDescription) | Data |  |

<a name="TripDescription"></a>
**Trip Description**

| Type | Name | Description |
|---|---|---|
| int | Id | |
| string | StartLocation |  |
| List of decimals | StartPosition |  |
| DateTime | StartLocalTimestamp |  |
| string | EndLocation |  |
| List of decimals | EndPosition |  |
| DateTime | EndLocalTimestamp |  |
| decimal | Distance |  |
| int | MaxSpeed |  |
| int | NumberOfExceptions |  |
| int | NumberOfDriverBehaviourExceptions |  |
| bool | IsBusiness |  |
| List of [Trip Event descriptions](#TripEventDescription) | TripEvent |  |

<a name="TripEventDescription"></a>
**Trip Event Description**

| Type | Name | Description |
|---|---|---|
| List of decimals | P | Latitude and longitude |
| DateTime | T | Event timestamp |
| int | E | Event type identifier |


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
| URL | ~/api/VehicleMakeModels |
| Method | GET |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |

| Parameters |  |
|---|---|
| dataCode | Make-Model code  |


**Response Object**

| Type | Name | Description |
|---|---|---|
| int | ObdPortLocationId | instalation location ID |
| string | DataCode | New Make-Model code |
| int | YearIntroduced | Vehicle introduction year |
| int | YearDiscontinued | Year of the end of vehicle manufactoring  |
| string | MakeDescription | Vehicle make |
| string | ModelDescription |Vehicle model |

Vehicles - ChangeVin
------------------
| **** | **** |
|---|---|
| Description | Change VIN for Vehicle |
| URL | ~/api/Vehicles/ChangeVin |
| Method | POST |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | HTTP Status Code |


**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | CurrentNumber| yes | Current VIN number |    
| string | NewNumber | yes | New VIN number |        
| bool | ChangeOnlyInPortal | false | Change only in Portal (Default: false) |

**Response Object**

Empty

Vehicles - OData Patch
-----------------

| **** | **** |
|---|---|
| Description | Update Vehicle data |
| URL | ~/api/Vehicles |
| Method | PATCH |
| Authorize | Administrator |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |

| Parameters |  |
|---|---|
| id | Vehicle ID  |

**Request Object**

| Type | Name | Required| Description |
|---|---|---|---|
| string | MakeModelCode | no | New Make-Model code  |    
| int | YearOfInitialRegistration | no | Year of vehicle initial registration | 
| string | Make | no | Vehicle make | 
| string | Model | no | Vehicle model | 
| string | LicensePlate | no |  |

**Response Object**

| Name | Type | Description |
|---|---|---|
| Id | int |  |  |
| VIN | string | VIN or Insurer specific unique vehicle identifier| 
| HSN | string | Herstellerschlüsselnummer - germany specific vehicle make/model identifier  |
| TSN | string | Typschlüsselnummer - germany specific vehicle make/model identifier |
| MakeModelCode | string | Insurer specific make model code | 
| LicensePlate | string | | 
| Color |  string | | 
| Make |  string | | 
| Model |  string | | 
| YearOfInitialRegistration |  int | 
| MotorType |  string | | 


UserPlaces - Get
------------------
Gets a list of all user-created places. Only places accessible by current user are returned.

| **** | **** |
|---|---|
| URL | ~/api/UserPlaces |
| Method | GET |
| Authorize | User |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |
| Response Codes | 200 - OK |

**Response Object** - object array of UserPlaceItem

| Type | Name | Description |
|---|---|---|
| int | Id | User place ID |
| string | Name | Name of the user place |
| string | Address | Address of the user place |
| string | Geometry | GeoJSON object * |

Note [GeoJSON object](https://tools.ietf.org/html/rfc7946#section-3) type is limited to Polygon. Coordinates are in WGS 84 (also known as EPSG:4326) coordinate reference system.

```
Example of UserPlaceItem
{
        "Id": 111,
        "Name": "Office",
        "Address": "Some address",
        "Geometry": {
            "type": "Polygon",
            "coordinates": [
                [
                    [
                        24.131401269074512,
                        56.996303940202829
                    ],
                    [
                        24.134010187009356,
                        56.996179586042864
                    ],
		    ...
		    [
                        34.906665235400467,
                        32.529426377258829
                    ]
                ]
            ]
        }
    }
```

UserPlaces - Get by id
------------------
Gets user-created place by Id. Only places accessible by current user are available.

| **** | **** |
|---|---|
| URL | ~/api/UserPlaces/{id} |
| Method | GET |
| Authorize | User |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |
| Response Codes | 200 - OK, 404 - Not Found |

| Parameters |  |
|---|---|
| id | User Place ID  |

**Response Object** - object UserPlaceItem

| Type | Name | Description |
|---|---|---|
| int | Id | User place ID |
| string | Name | Name of the user place |
| string | Address | Address of the user place |
| string | Geometry | GeoJSON object * |

Note [GeoJSON object](https://tools.ietf.org/html/rfc7946#section-3) type is limited to Polygon. Coordinates are in WGS 84 (also known as EPSG:4326) coordinate reference system.

UserPlaces - POST
------------------
Creates a user place

| **** | **** |
|---|---|
| URL | ~/api/UserPlaces |
| Method | POST |
| Authorize | User |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object with created user place. |
| Response Codes | 200 - OK, 400 - Bad Request |

**Request Object**

| Type | Name | Description |
|---|---|---|
| string | Name | Name of the user place |
| string | Address | Address of the user place |
| string | Geometry | Json-formatted Geometry object  |

**Response Object** - object UserPlaceItem

| Type | Name | Description |
|---|---|---|
| int | Id | User place ID |
| string | Name | Name of the user place |
| string | Address | Address of the user place |
| string | Geometry | Json-formatted Geometry object  |


UserPlaces - PATCH
------------------
Updates the properties of a user-created place

| **** | **** |
|---|---|
| URL | ~/api/UserPlaces/{id} |
| Method | PATCH |
| Authorize | User |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |
| Response Codes | 200 - OK, 404 - Not Found, 400 - Bad Request |

| Parameters |  |
|---|---|
| id | User Place ID  |

**Request Object**

| Type | Name | Description |
|---|---|---|
| string | Name | Name of the user place |
| string | Address | Address of the user place |
| string | Geometry | Json-formatted Geometry object  |

**Response Object** - object UserPlaceItem

| Type | Name | Description |
|---|---|---|
| int | Id | User place ID |
| string | Name | Name of the user place |
| string | Address | Address of the user place |
| string | Geometry | Json-formatted Geometry object  |

UserPlaces - DELETE
------------------
Deletes a user-created place

| **** | **** |
|---|---|
| URL | ~/api/UserPlaces/{id} |
| Method | DELETE |
| Authorize | User |
| Response Content-Type | application/json; charset=utf-8 |
| Response |  |
| Response Codes | 204 - Deleted, 404 - Not Found |

| Parameters |  |
|---|---|
| id | User Place ID  |


RuleInstances - GET
------------------
Gets the list of rules, optionally filtered by ruleTypeId. If policyVehicleUnitId is not specified, returns all RuleInstances that the logged on user has created. If policyVehicleUnitId is specified, returns all RuleInstances for that PolicyVehicleUnit, regardless which user created them.

| **** | **** |
|---|---|
| URL |~/api/RuleInstances?ruleTypeId={ruleTypeId}&policyVehicleUnitId={policyVehicleUnitId} |
| Method | GET |
| Authorize | Administrator, User |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object |
| Response Codes | 200 - Ok |

| Parameters |  |
|---|---|
| ruleTypeId | (optional) rule type name from RuleType enumeration f.e. PlaceEntry |
| policyVehicleUnitId | (optional) returns all rule instances created for that policyVehicleUnitId, regardless which user created them |


**Response Object** - array of RuleInstanceItem

| Type | Name | Description |
|---|---|---|
| int | Id | RuleInstance ID |
| int | PolicyVehicleUnitId | PVU Id |
| [RuleType](RuleType) | RuleType | Type of the rule f.e. "PlaceEntry" |
| string | Params | Rule definition parameters that depends on RuleType  |
| bool | IsPushRequired | If push notification to end-user device is requred  |
| bool | IsEmailRequired | If email notification to end-user is requred  |
| bool | IsRepeating | If email notification to end-user is requred  |
| bool | IsEnabled | If email notification to end-user is requred  |

```
Example of RuleInstanceItem

 {
        "Id": 123,
        "PolicyVehicleUnitId": 45678,
        "RuleType": "PlaceEntry",
        "Params": {
            "placeId": 274,
            "cooldown": 900
        },
        "IsPushRequired": true,
        "IsEmailRequired": true,
        "IsRepeating": false,
        "IsEnabled": true
    },
```

RuleInstances - POST
------------------
Creates a rule instance of the specified rule type

| **** | **** |
|---|---|
| URL | ~/api/RuleInstances |
| Method | POST |
| Authorize | User |
| Response Content-Type | application/json; charset=utf-8 |
| Response | JSON object with created rule instance |
| Response Codes | 200 - OK, 400 - Bad Request, 404 - Subscription Not Found |

**Request Object** - object RuleInstance

| Type | Name | Description |
|---|---|---|
| int | PolicyVehicleUnitId | PVU Id |
| [RuleType](RuleType) | RuleType | Type of the rule f.e. "PlaceEntry" |
| string | Params | Rule definition parameters that depends on RuleType |
| bool | IsPushRequired | If push notification to end-user device is requred  |
| bool | IsEmailRequired | If email notification to end-user is requred  |
| bool | IsRepeating | If email notification to end-user is requred  |
| bool | IsEnabled | If email notification to end-user is requred  |

Note: "params" attribute is an JSON object whose structure depends on rule type. Object structure for rule types of all use cases is given below.

```
Parameters example for RuleTypes: PlaceEntry = 1, PlaceExit = 2, DoNotEnterPlace = 3, DoNotExitPlace = 4

"Params": {
	"placeId": 17, /* ID of the user place to monitor */
	"timeProfile": { /* optional, specifies when rule is active. */
		"entries": [
			{
				"dayOfWeekUtc": 4, // Mon=1, Tue=2, etc.
				"startTimeUtc": 48600, // Block start, in seconds since midnight (UTC)
				"endTimeUtc": 49500 // Block end, in seconds since midnight (UTC)
			},
			{
				"dayOfWeekUtc": 3, // Mon=1, Tue=2, etc.
				"startTimeUtc": 0, // Seconds since midnight
				"endTimeUtc": 900 // Seconds since midnight
			}
		]
	},
	"cooldown": 900 // optional, time period in seconds during which the rule is not retriggered
}

Parameters example for RuleTypes: MustLeaveByTimeRange = 5
"Params": {
{
	"placeId": 17, /* place to monitor */
	"interval": 60, /* periodic interval. Aligned (relative) to seconds since midnight UTC. */
	"timeProfile": { /* optional, specifies when rule is active. */
		"entries": [
			{
				"dayOfWeekUtc": 3, // Mon=1, Tue=2, etc.
				"startTimeUtc": 0, // Seconds since midnight
				"endTimeUtc": 900 // Seconds since midnight
			}
		]
	},
	"cooldown": 900 // time period in seconds during which the rule is not retriggered
}

Parameters example for RuleTypes: ArriveInGivenTime = 6

"Params": {
	"departurePlaceId": 17 /* user place id to exit to begin arrival countdown */
	"interval": 1800 /* number of seconds after which the vehicle should be in the destination place */
	"destinationPlaceId": 18 /* place where the vehicle should be after given time */
	"timeProfile": { /* optional, specifies when rule is active. */
		"entries": [
			{
				"dayOfWeekUtc": 3, // Mon=1, Tue=2, etc.
				"startTimeUtc": 28800, // Seconds since midnight
				"endTimeUtc": 61200 // Seconds since midnight
			}
		]
	},
	"cooldown": 900 // time period in seconds during which the rule is not retriggered
}
```
	
**Response Object** - object RuleInstanceResponse

| Type | Name | Description |
|---|---|---|
| int | Id | Rule Id |
| int | PolicyVehicleUnitId | PVU Id |
| [RuleType](RuleType) | RuleType | Type of the rule f.e. "PlaceEntry" |
| string | Params | Rule definition parameters that depends on RuleType |
| bool | IsPushRequired | If push notification to end-user device is requred  |
| bool | IsEmailRequired | If email notification to end-user is requred  |
| bool | IsRepeating | If email notification to end-user is requred  |
| bool | IsEnabled | If email notification to end-user is requred  |

