# Contents of Amazon SES Event Data Published to Amazon SNS<a name="event-publishing-retrieving-sns-contents"></a>

Amazon SES publishes email sending event records to Amazon Simple Notification Service in JSON format\.

The top\-level JSON object contains an `eventType` string, a `mail` object, and either a `bounce`, `complaint`, `delivery`, `send`, `reject`, `open`, `click`, or `failure` object, depending on the type of event\.


+ [Top\-Level JSON Object](#event-publishing-retrieving-sns-contents-top-level-json-object)
+ [Mail Object](#event-publishing-retrieving-sns-contents-mail-object)
+ [Bounce Object](#event-publishing-retrieving-sns-contents-bounce-object)
+ [Complaint Object](#event-publishing-retrieving-sns-contents-complaint-object)
+ [Delivery Object](#event-publishing-retrieving-sns-contents-delivery-object)
+ [Send Object](#event-publishing-retrieving-sns-contents-send-object)
+ [Reject Object](#event-publishing-retrieving-sns-contents-reject-object)
+ [Open Object](#event-publishing-retrieving-sns-contents-open-object)
+ [Click Object](#event-publishing-retrieving-sns-contents-click-object)
+ [Failure Object](#event-publishing-retrieving-sns-contents-failure-object)

## Top\-Level JSON Object<a name="event-publishing-retrieving-sns-contents-top-level-json-object"></a>

The top\-level JSON object in an email sending event record contains the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `eventType`  |  A string that describes the type of event\. Possible values: `Delivery`, `Send`, `Reject`, `Open`, `Click`, `Bounce`, `Complaint`, or `Rendering Failure`\.  | 
|  `mail`  |  A JSON object that contains information about the email that produced the event\.  | 
|  `bounce`  |  This field is only present if `eventType` is `Bounce`\. It contains information about the bounce\.  | 
|  `complaint`  |  This field is only present if `eventType` is `Complaint`\. It contains information about the complaint\.  | 
|  `delivery`  |  This field is only present if `eventType` is `Delivery`\. It contains information about the delivery\.  | 
|  `send`  |  This field is only present if `eventType` is `Send`\.  | 
|  `reject`  |  This field is only present if `eventType` is `Reject`\. It contains information about the rejection\.  | 
|  `open`  |  This field is only present if `eventType` is `Open`\. It contains information about the open event\.  | 
|  `click`  |  This field is only present if `eventType` is `Click`\. It contains information about the click event\.  | 
| `failure` | This field is only present if `eventType` is `Rendering Failure`\. It contains information about the rendering failure event\. | 

## Mail Object<a name="event-publishing-retrieving-sns-contents-mail-object"></a>

Each email sending event record contains information about the original email in the `mail` object\. The JSON object that contains information about a `mail` object has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `timestamp`  |  The date and time, in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\), when the message was sent\.  | 
|  `messageId`  |  A unique ID that Amazon SES assigned to the message\. Amazon SES returned this value to you when you sent the message\.  This message ID was assigned by Amazon SES\. You can find the message ID of the original email in the `headers` and `commonHeaders` fields of the `mail` object\.   | 
|  `source`  |  The email address that the message was sent from \(the envelope MAIL FROM address\)\.  | 
|  `sourceArn`  |  The Amazon Resource Name \(ARN\) of the identity that was used to send the email\. In the case of sending authorization, the `sourceArn` is the ARN of the identity that the identity owner authorized the delegate sender to use to send the email\. For more information about sending authorization, see [Using Sending Authorization](sending-authorization.md)\.  | 
|  `sendingAccountId`  |  The AWS account ID of the account that was used to send the email\. In the case of sending authorization, the `sendingAccountId` is the delegate sender's account ID\.  | 
|  `destination`  |  A list of email addresses that were recipients of the original mail\.  | 
|  `headersTruncated`  |  A string that specifies whether the headers are truncated in the notification, which occurs if the headers are larger than 10 KB\. Possible values are `true` and `false`\.  | 
|  `headers`  |  A list of the email's original headers\. Each header in the list has a `name` field and a `value` field\.  Any message ID within the `headers` field is from the original message that you passed to Amazon SES\. The message ID that Amazon SES subsequently assigned to the message is in the `messageId` field of the `mail` object\.   | 
|  `commonHeaders`  |  A list of the email's original, commonly used headers\. Each header in the list has a `name` field and a `value` field\.  Any message ID within the `commonHeaders` field is from the original message that you passed to Amazon SES\. The message ID that Amazon SES subsequently assigned to the message is in the `messageId` field of the `mail` object\.   | 

## Bounce Object<a name="event-publishing-retrieving-sns-contents-bounce-object"></a>

The JSON object that contains information about a `Bounce` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `bounceType`  |  The type of bounce, as determined by Amazon SES\.  | 
|  `bounceSubType`  |  The subtype of the bounce, as determined by Amazon SES\.  | 
|  `bouncedRecipients`  |  A list that contains information about the recipients of the original mail that bounced\.  | 
|  `timestamp`  |  The date and time, in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\), when the ISP sent the bounce notification\.  | 
|  `feedbackId`  |  A unique ID for the bounce\.  | 
|  `reportingMTA`  |  The value of the `Reporting-MTA` field from the DSN\. This is the value of the Message Transfer Authority \(MTA\) that attempted to perform the delivery, relay, or gateway operation described in the DSN\.  This field only appears if a delivery status notification \(DSN\) was attached to the bounce\.   | 

### Bounced Recipients<a name="event-publishing-retrieving-sns-contents-bounced-recipients"></a>

A bounce event may pertain to a single recipient or to multiple recipients\. The `bouncedRecipients` field holds a list of objects—one object per recipient whose email address produced a bounce—and contains the following field\.


| Field Name | Description | 
| --- | --- | 
|  `emailAddress`  |  The email address of the recipient\. If a DSN is available, this is the value of the `Final-Recipient` field from the DSN\.  | 

Optionally, if a DSN is attached to the bounce, the following fields may also be present\.


| Field Name | Description | 
| --- | --- | 
|  `action`  |  The value of the `Action` field from the DSN\. This indicates the action performed by the reporting MTA as a result of its attempt to deliver the message to this recipient\.  | 
|  `status`  |  The value of the `Status` field from the DSN\. This is the per\-recipient transport\-independent status code that indicates the delivery status of the message\.  | 
|  `diagnosticCode`  |  The status code issued by the reporting MTA\. This is the value of the `Diagnostic-Code` field from the DSN\. This field may be absent in the DSN \(and therefore also absent in the JSON\)\.  | 

### Bounce Types<a name="event-publishing-retrieving-sns-contents-bounce-types"></a>

Each bounce event is of one of the types shown in the following table\.

The event publishing system only publishes hard bounces and soft bounces that are no longer retried by Amazon SES\. When you receive bounces marked `Permanent`, you should remove the corresponding email addresses from your mailing list; you will not be able to send to them in the future\. `Transient` bounces are sent to you when a message has soft bounced several times, and Amazon SES has stopped trying to re\-deliver it\. You may be able to successfully resend to an address that initially resulted in a `Transient` bounce in the future\.


| bounceType | bounceSubType | Description | 
| --- | --- | --- | 
|  `Undetermined`  |  `Undetermined`  |  Amazon SES was unable to determine a specific bounce reason\.  | 
|  `Permanent`  |  `General`  |  Amazon SES received a general hard bounce\. If you receive this type of bounce, you should remove the recipient's email address from your mailing list\.  | 
|  `Permanent`  |  `NoEmail`  |  Amazon SES received a permanent hard bounce because the target email address does not exist\. If you receive this type of bounce, you should remove the recipient's email address from your mailing list\.  | 
|  `Permanent`  |  `Suppressed`  |  Amazon SES has suppressed sending to this address because it has a recent history of bouncing as an invalid address\. For information about how to remove an address from the suppression list, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.  | 
|  `Transient`  |  `General`  |  Amazon SES received a general bounce\. You may be able to successfully send to this recipient in the future\.  | 
|  `Transient`  |  `MailboxFull`  |  Amazon SES received a mailbox full bounce\. You may be able to successfully send to this recipient in the future\.  | 
|  `Transient`  |  `MessageTooLarge`  |  Amazon SES received a message too large bounce\. You may be able to successfully send to this recipient if you reduce the size of the message\.  | 
|  `Transient`  |  `ContentRejected`  |  Amazon SES received a content rejected bounce\. You may be able to successfully send to this recipient if you change the content of the message\.  | 
|  `Transient`  |  `AttachmentRejected`  |  Amazon SES received an attachment rejected bounce\. You may be able to successfully send to this recipient if you remove or change the attachment\.  | 

## Complaint Object<a name="event-publishing-retrieving-sns-contents-complaint-object"></a>

The JSON object that contains information about a `Complaint` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `complainedRecipients`  |  A list that contains information about recipients that may have submitted the complaint\.  | 
|  `timestamp`  |  The date and time, in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\), when the ISP sent the complaint notification\.  | 
|  `feedbackId`  |  A unique ID for the complaint\.  | 

Further, if a feedback report is attached to the complaint, the following fields may be present\.


| Field Name | Description | 
| --- | --- | 
|  `userAgent`  |  The value of the `User-Agent` field from the feedback report\. This indicates the name and version of the system that generated the report\.  | 
|  `complaintFeedbackType`  |  The value of the `Feedback-Type` field from the feedback report received from the ISP\. This contains the type of feedback\.  | 
|  `arrivalDate`  |  The value of the `Arrival-Date` or `Received-Date` field from the feedback report in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\)\. This field may be absent in the report \(and therefore also absent in the JSON\)\.  | 

### Complained Recipients<a name="event-publishing-retrieving-sns-contents-complained-recipients"></a>

The `complainedRecipients` field contains a list of recipients that may have submitted the complaint\. 

**Important**  
Most ISPs redact the email addresses of recipients who submit complaints\. For this reason, the `complainedRecipients` field includes a list of everyone who was sent the email whose address is on the domain that issued the complaint notification\.

JSON objects in this list contain the following field\.


| Field Name | Description | 
| --- | --- | 
|  `emailAddress`  |  The email address of the recipient\.  | 

### Complaint Types<a name="event-publishing-retrieving-sns-contents-complaint-types"></a>

You may see the following complaint types in the `complaintFeedbackType` field as assigned by the reporting ISP, according to the [Internet Assigned Numbers Authority website](https://www.iana.org/assignments/marf-parameters/marf-parameters.xml#marf-parameters-2):


| Field Name | Description | 
| --- | --- | 
|  `abuse`  |  Indicates unsolicited email or some other kind of email abuse\.  | 
|  `auth-failure`  |  Email authentication failure report\.  | 
|  `fraud`  |  Indicates some kind of fraud or phishing activity\.  | 
|  `not-spam`  |  Indicates that the entity providing the report does not consider the message to be spam\. This may be used to correct a message that was incorrectly tagged or categorized as spam\.  | 
|  `other`  |  Indicates any other feedback that does not fit into other registered types\.  | 
|  `virus`  |  Reports that a virus is found in the originating message\.  | 

## Delivery Object<a name="event-publishing-retrieving-sns-contents-delivery-object"></a>

The JSON object that contains information about a `Delivery` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `timestamp`  |  The date and time when Amazon SES delivered the email to the recipient's mail server, in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\)\.  | 
|  `processingTimeMillis`  |  The time in milliseconds between when Amazon SES accepted the request from the sender to when Amazon SES passed the message to the recipient's mail server\.  | 
|  `recipients`  |  A list of intended recipients that the delivery event applies to\.  | 
|  `smtpResponse`  |  The SMTP response message of the remote ISP that accepted the email from Amazon SES\. This message will vary by email, by receiving mail server, and by receiving ISP\.  | 
|  `reportingMTA`  |  The host name of the Amazon SES mail server that sent the mail\.  | 

## Send Object<a name="event-publishing-retrieving-sns-contents-send-object"></a>

The JSON object that contains information about a `send` event is always empty\.

## Reject Object<a name="event-publishing-retrieving-sns-contents-reject-object"></a>

The JSON object that contains information about a `Reject` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `reason`  |  The reason the email was rejected\. The only possible value is `Bad content`, which means that Amazon SES detected that the email contained a virus\.  | 

## Open Object<a name="event-publishing-retrieving-sns-contents-open-object"></a>

The JSON object that contains information about a `Open` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `ipAddress`  |  The recipient's IP address\.  | 
|  `timestamp`  |  The date and time when the open event occurred in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\)\.  | 
|  `userAgent`  |  The user agent of the device or email client that the recipient used to open the email\.  | 

## Click Object<a name="event-publishing-retrieving-sns-contents-click-object"></a>

The JSON object that contains information about a `Click` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `ipAddress`  |  The recipient's IP address\.  | 
|  `timestamp`  |  The date and time when the click event occurred in ISO8601 format \(*YYYY\-MM\-DDThh:mm:ss\.sZ*\)\.  | 
|  `userAgent`  |  The user agent of the client that the recipient used to click a link in the email\.  | 
|  `link`  |  The URL of the link that the recipient clicked\.  | 
|  `linkTags`  |  A list of tags that were added to the link using the `ses:tags` attribute\. For more information about adding tags to links in your emails, see [Q5\. Can I tag links with unique identifiers?](sending-metric-faqs.md#sending-metric-faqs-clicks-q5) in the [Amazon SES Email Sending Metrics FAQs](sending-metric-faqs.md)\.  | 

## Failure Object<a name="event-publishing-retrieving-sns-contents-failure-object"></a>

The JSON object that contains information about a `Rendering Failure` event has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `templateName`  |  The name of the template used to send the email\.  | 
|  `errorMessage`  |  A message that provides more information about the rendering failure\.  | 