---
title: "Open Match API References" 
linkTitle: "Open Match API References" 
weight: 2 
description: 
  This document provides API references for Open Match services. 
--- 

# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [api/backend.proto](#api/backend.proto)
    - [AssignTicketsRequest](#openmatch.AssignTicketsRequest)
    - [AssignTicketsResponse](#openmatch.AssignTicketsResponse)
    - [FetchMatchesRequest](#openmatch.FetchMatchesRequest)
    - [FetchMatchesResponse](#openmatch.FetchMatchesResponse)
    - [FunctionConfig](#openmatch.FunctionConfig)
  
    - [FunctionConfig.Type](#openmatch.FunctionConfig.Type)
  
  
    - [Backend](#openmatch.Backend)
  

- [api/evaluator.proto](#api/evaluator.proto)
    - [EvaluateRequest](#openmatch.EvaluateRequest)
    - [EvaluateResponse](#openmatch.EvaluateResponse)
  
  
  
    - [Evaluator](#openmatch.Evaluator)
  

- [api/extensions.proto](#api/extensions.proto)
    - [DefaultEvaluationCriteria](#openmatch.DefaultEvaluationCriteria)
  
  
  
  

- [api/frontend.proto](#api/frontend.proto)
    - [CreateTicketRequest](#openmatch.CreateTicketRequest)
    - [CreateTicketResponse](#openmatch.CreateTicketResponse)
    - [DeleteTicketRequest](#openmatch.DeleteTicketRequest)
    - [DeleteTicketResponse](#openmatch.DeleteTicketResponse)
    - [GetAssignmentsRequest](#openmatch.GetAssignmentsRequest)
    - [GetAssignmentsResponse](#openmatch.GetAssignmentsResponse)
    - [GetTicketRequest](#openmatch.GetTicketRequest)
  
  
  
    - [Frontend](#openmatch.Frontend)
  

- [api/matchfunction.proto](#api/matchfunction.proto)
    - [RunRequest](#openmatch.RunRequest)
    - [RunResponse](#openmatch.RunResponse)
  
  
  
    - [MatchFunction](#openmatch.MatchFunction)
  

- [api/messages.proto](#api/messages.proto)
    - [Assignment](#openmatch.Assignment)
    - [DoubleRangeFilter](#openmatch.DoubleRangeFilter)
    - [Match](#openmatch.Match)
    - [MatchProfile](#openmatch.MatchProfile)
    - [Pool](#openmatch.Pool)
    - [Roster](#openmatch.Roster)
    - [SearchFields](#openmatch.SearchFields)
    - [SearchFields.DoubleArgsEntry](#openmatch.SearchFields.DoubleArgsEntry)
    - [SearchFields.StringArgsEntry](#openmatch.SearchFields.StringArgsEntry)
    - [StringEqualsFilter](#openmatch.StringEqualsFilter)
    - [TagPresentFilter](#openmatch.TagPresentFilter)
    - [Ticket](#openmatch.Ticket)
  
  
  
  

- [api/mmlogic.proto](#api/mmlogic.proto)
    - [QueryTicketsRequest](#openmatch.QueryTicketsRequest)
    - [QueryTicketsResponse](#openmatch.QueryTicketsResponse)
  
  
  
    - [MmLogic](#openmatch.MmLogic)
  

- [Scalar Value Types](#scalar-value-types)



<a name="api/backend.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/backend.proto



<a name="openmatch.AssignTicketsRequest"></a>

### AssignTicketsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ticket_ids | [string](#string) | repeated | TicketIds is a list of strings representing Open Match generated Ids which apply to an Assignment. |
| assignment | [Assignment](#openmatch.Assignment) |  | An Assignment specifies game connection related information to be associated with the TicketIds. |






<a name="openmatch.AssignTicketsResponse"></a>

### AssignTicketsResponse







<a name="openmatch.FetchMatchesRequest"></a>

### FetchMatchesRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| config | [FunctionConfig](#openmatch.FunctionConfig) |  | FunctionConfig specifies a MMF address and client type for Backend to establish connections with the MMF |
| profiles | [MatchProfile](#openmatch.MatchProfile) | repeated | MatchProfiles that will be sent to thhe MMF specified in the FunctionConfig. |






<a name="openmatch.FetchMatchesResponse"></a>

### FetchMatchesResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| match | [Match](#openmatch.Match) |  | A Match generated by the user-defined MMF with the specified MatchProfiles. A valid Match response will contain at least one ticket. |






<a name="openmatch.FunctionConfig"></a>

### FunctionConfig
FunctionConfig specifies a MMF address and client type for Backend to establish connections with the MMF


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| host | [string](#string) |  |  |
| port | [int32](#int32) |  |  |
| type | [FunctionConfig.Type](#openmatch.FunctionConfig.Type) |  |  |





 


<a name="openmatch.FunctionConfig.Type"></a>

### FunctionConfig.Type


| Name | Number | Description |
| ---- | ------ | ----------- |
| GRPC | 0 |  |
| REST | 1 |  |


 

 


<a name="openmatch.Backend"></a>

### Backend
The Backent service implements APIs to generate matches and handle ticket assignments.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| FetchMatches | [FetchMatchesRequest](#openmatch.FetchMatchesRequest) | [FetchMatchesResponse](#openmatch.FetchMatchesResponse) stream | FetchMatches triggers a MatchFunction with the specified MatchProfiles, while each MatchProfile returns a set of match proposals. FetchMatches method streams the results back to the caller. FetchMatches immediately returns an error if it encounters any execution failures. - If the synchronizer is enabled, FetchMatch will then call the synchronizer to deduplicate proposals with overlapped tickets. |
| AssignTickets | [AssignTicketsRequest](#openmatch.AssignTicketsRequest) | [AssignTicketsResponse](#openmatch.AssignTicketsResponse) | AssignTickets overwrites the Assignment field of the input TicketIds. |

 



<a name="api/evaluator.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/evaluator.proto



<a name="openmatch.EvaluateRequest"></a>

### EvaluateRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| match | [Match](#openmatch.Match) |  | A Matches proposed by the Match Function representing a candidate of the final results. |






<a name="openmatch.EvaluateResponse"></a>

### EvaluateResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| match | [Match](#openmatch.Match) |  | A Match shortlisted by the evaluator representing one of the final results. |





 

 

 


<a name="openmatch.Evaluator"></a>

### Evaluator
The Evaluator service implements APIs used to evaluate and shortlist matches proposed by MMFs.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| Evaluate | [EvaluateRequest](#openmatch.EvaluateRequest) stream | [EvaluateResponse](#openmatch.EvaluateResponse) stream | Evaluate evaluates a list of proposed matches based on quality, collision status, and etc, then shortlist the matches and returns the final results. |

 



<a name="api/extensions.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/extensions.proto



<a name="openmatch.DefaultEvaluationCriteria"></a>

### DefaultEvaluationCriteria
A DefaultEvaluationCriteria is used for a match&#39;s evaluation_input when using
the default evaluator.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| score | [double](#double) |  |  |





 

 

 

 



<a name="api/frontend.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/frontend.proto



<a name="openmatch.CreateTicketRequest"></a>

### CreateTicketRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ticket | [Ticket](#openmatch.Ticket) |  | A Ticket object with SearchFields defined. |






<a name="openmatch.CreateTicketResponse"></a>

### CreateTicketResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ticket | [Ticket](#openmatch.Ticket) |  | A Ticket object with TicketId generated. |






<a name="openmatch.DeleteTicketRequest"></a>

### DeleteTicketRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ticket_id | [string](#string) |  | A TicketId of a generated Ticket to be deleted. |






<a name="openmatch.DeleteTicketResponse"></a>

### DeleteTicketResponse







<a name="openmatch.GetAssignmentsRequest"></a>

### GetAssignmentsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ticket_id | [string](#string) |  | A TicketId of a generated Ticket to get updates on. |






<a name="openmatch.GetAssignmentsResponse"></a>

### GetAssignmentsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| assignment | [Assignment](#openmatch.Assignment) |  | An updated Assignment of the requested Ticket. |






<a name="openmatch.GetTicketRequest"></a>

### GetTicketRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ticket_id | [string](#string) |  | A TicketId of a generated Ticket. |





 

 

 


<a name="openmatch.Frontend"></a>

### Frontend
The Frontend service implements APIs to manage and query status of a Tickets.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| CreateTicket | [CreateTicketRequest](#openmatch.CreateTicketRequest) | [CreateTicketResponse](#openmatch.CreateTicketResponse) | CreateTicket assigns an unique TicketId to the input Ticket and record it in state storage. A ticket is considered as ready for matchmaking once it is created. - If a TicketId exists in a Ticket request, an auto-generated TicketId will override this field. - If SearchFields exist in a Ticket, CreateTicket will also index these fields such that one can query the ticket with mmlogic.QueryTickets function. |
| DeleteTicket | [DeleteTicketRequest](#openmatch.DeleteTicketRequest) | [DeleteTicketResponse](#openmatch.DeleteTicketResponse) | DeleteTicket immediately stops Open Match from using the Ticket for matchmaking and removes the Ticket from state storage. The client must delete the Ticket when finished matchmaking with it. - If SearchFields exist in a Ticket, DeleteTicket will deindex the fields lazily. Users may still be able to assign/get a ticket after calling DeleteTicket on it. |
| GetTicket | [GetTicketRequest](#openmatch.GetTicketRequest) | [Ticket](#openmatch.Ticket) | GetTicket get the Ticket associated with the specified TicketId. |
| GetAssignments | [GetAssignmentsRequest](#openmatch.GetAssignmentsRequest) | [GetAssignmentsResponse](#openmatch.GetAssignmentsResponse) stream | GetAssignments stream back Assignment of the specified TicketId if it is updated. - If the Assignment is not updated, GetAssignment will retry using the configured backoff strategy. |

 



<a name="api/matchfunction.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/matchfunction.proto



<a name="openmatch.RunRequest"></a>

### RunRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| profile | [MatchProfile](#openmatch.MatchProfile) |  | A MatchProfile defines constraints of Tickets in a Match and shapes the Match proposed by the MatchFunction. |






<a name="openmatch.RunResponse"></a>

### RunResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| proposal | [Match](#openmatch.Match) |  | A Proposal represents a Match candidate that satifies the constraints defined in the input Profile. A valid Proposal response will contain at least one ticket. |





 

 

 


<a name="openmatch.MatchFunction"></a>

### MatchFunction
The MatchFunction service implements APIs to run user-defined matchmaking logics.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| Run | [RunRequest](#openmatch.RunRequest) | [RunResponse](#openmatch.RunResponse) stream | DO NOT CALL THIS FUNCTION MANUALLY. USE backend.FetchMatches INSTEAD. Run pulls Tickets that satisify Profile constraints from Mmlogic, runs matchmaking logics against them, then constructs and streams back match candidates to the Backend service. |

 



<a name="api/messages.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/messages.proto



<a name="openmatch.Assignment"></a>

### Assignment
An Assignment represents a game server assignment associated with a Ticket. Open
match does not require or inspect any fields on assignment.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| connection | [string](#string) |  | Connection information for this Assignment. |
| error | [google.rpc.Status](#google.rpc.Status) |  | Error when finding an Assignment for this Ticket. |
| extension | [google.protobuf.Any](#google.protobuf.Any) |  | Customized information to be sent to the clients. Optional, depending on what callers are expecting. |






<a name="openmatch.DoubleRangeFilter"></a>

### DoubleRangeFilter
Filters numerical values to only those within a range.
  double_arg: &#34;foo&#34;
  max: 10
  min: 5
matches:
  {&#34;foo&#34;: 5}
  {&#34;foo&#34;: 7.5}
  {&#34;foo&#34;: 10}
does not match:
  {&#34;foo&#34;: 4}
  {&#34;foo&#34;: 10.01}
  {&#34;foo&#34;: &#34;7.5&#34;}
  {}


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| double_arg | [string](#string) |  | Name of the ticket&#39;s search_fields.double_args this Filter operates on. |
| max | [double](#double) |  | Maximum value. Defaults to positive infinity (any value above minv). |
| min | [double](#double) |  | Minimum value. Defaults to 0. |






<a name="openmatch.Match"></a>

### Match
A Match is used to represent a completed match object. It can be generated by
a MatchFunction as a proposal or can be returned by OpenMatch as a result in
response to the FetchMatches call.
When a match is returned by the FetchMatches call, it should contain at least 
one ticket to be considered as valid.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| match_id | [string](#string) |  | A Match ID that should be passed through the stack for tracing. |
| match_profile | [string](#string) |  | Name of the match profile that generated this Match. |
| match_function | [string](#string) |  | Name of the match function that generated this Match. |
| tickets | [Ticket](#openmatch.Ticket) | repeated | Tickets belonging to this match. |
| rosters | [Roster](#openmatch.Roster) | repeated | Set of Rosters that comprise this Match |
| evaluation_input | [google.protobuf.Any](#google.protobuf.Any) |  | Customized information for the evaluator. Optional, depending on the requirements of the configured evaluator. |
| extension | [google.protobuf.Any](#google.protobuf.Any) |  | Customized information for how the caller of FetchMatches should handle this match. Optional, depending on the requirements of the FetchMatches caller. |






<a name="openmatch.MatchProfile"></a>

### MatchProfile
A MatchProfile is Open Match&#39;s representation of a Match specification. It is
used to indicate the criteria for selecting players for a match. A
MatchProfile is the input to the API to get matches and is passed to the
MatchFunction. It contains all the information required by the MatchFunction
to generate match proposals.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | Name of this match profile. |
| pools | [Pool](#openmatch.Pool) | repeated | Set of pools to be queried when generating a match for this MatchProfile. The pool names can be used in empty Rosters to specify composition of a match. |
| rosters | [Roster](#openmatch.Roster) | repeated | Set of Rosters for this match request. Could be empty Rosters used to indicate the composition of the generated Match or they could be partially pre-populated Ticket list to be used in scenarios such as backfill / join in progress. |
| extension | [google.protobuf.Any](#google.protobuf.Any) |  | Customized information on how the match function should run. Optional, depending on the requirements of the match function. |






<a name="openmatch.Pool"></a>

### Pool



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | A developer-chosen human-readable name for this Pool. |
| double_range_filters | [DoubleRangeFilter](#openmatch.DoubleRangeFilter) | repeated | Set of Filters indicating the filtering criteria. Selected players must match every Filter. |
| string_equals_filters | [StringEqualsFilter](#openmatch.StringEqualsFilter) | repeated |  |
| tag_present_filters | [TagPresentFilter](#openmatch.TagPresentFilter) | repeated |  |






<a name="openmatch.Roster"></a>

### Roster
A Roster is a named collection of Ticket IDs. It exists so that a Tickets
associated with a Match can be labelled to belong to a team, sub-team etc. It
can also be used to represent the current state of a Match in scenarios such
as backfill, join-in-progress etc.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | A developer-chosen human-readable name for this Roster. |
| ticket_ids | [string](#string) | repeated | Tickets belonging to this Roster. |






<a name="openmatch.SearchFields"></a>

### SearchFields
Search fields are the fields which Open Match is aware of, and can be used
when specifying filters.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| double_args | [SearchFields.DoubleArgsEntry](#openmatch.SearchFields.DoubleArgsEntry) | repeated | Float arguments. Filterable on ranges. |
| string_args | [SearchFields.StringArgsEntry](#openmatch.SearchFields.StringArgsEntry) | repeated | String arguments. Filterable on equality. |
| tags | [string](#string) | repeated | Filterable on presence or absence of given value. |






<a name="openmatch.SearchFields.DoubleArgsEntry"></a>

### SearchFields.DoubleArgsEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [double](#double) |  |  |






<a name="openmatch.SearchFields.StringArgsEntry"></a>

### SearchFields.StringArgsEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |






<a name="openmatch.StringEqualsFilter"></a>

### StringEqualsFilter
Filters strings exactly equaling a value.
  string_arg: &#34;foo&#34;
  value: &#34;bar&#34;
matches:
  {&#34;foo&#34;: &#34;bar&#34;}
does not match:
  {&#34;foo&#34;: &#34;baz&#34;}
  {&#34;bar&#34;: &#34;foo&#34;}
  {}


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| string_arg | [string](#string) |  | Name of the ticket&#39;s search_fields.string_args this Filter operates on. |
| value | [string](#string) |  |  |






<a name="openmatch.TagPresentFilter"></a>

### TagPresentFilter
Filters to the tag being present on the search_fields.
  tag: &#34;foo&#34;
matches:
  [&#34;foo&#34;]
  [&#34;bar&#34;,&#34;foo&#34;]
does not match:
  [&#34;bar&#34;]
  []


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tag | [string](#string) |  |  |






<a name="openmatch.Ticket"></a>

### Ticket
A Ticket is a basic matchmaking entity in Open Match. A Ticket represents either an
individual &#39;Player&#39; or a &#39;Group&#39; of players. Open Match will not interpret
what the Ticket represents but just treat it as a matchmaking unit with a set
of SearchFields. Open Match stores the Ticket in state storage and enables an
Assignment to be associated with this Ticket.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  | Id represents an auto-generated Id issued by Open Match. |
| assignment | [Assignment](#openmatch.Assignment) |  | An Assignment represents a game server assignment associated with a Ticket. Open Match does not require or inspect any fields on Assignment. |
| search_fields | [SearchFields](#openmatch.SearchFields) |  | Search fields are the fields which Open Match is aware of, and can be used when specifying filters. |
| extension | [google.protobuf.Any](#google.protobuf.Any) |  | Customized information to be used by the Match Making Function. Optional, depending on the requirements of the MMF. |





 

 

 

 



<a name="api/mmlogic.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## api/mmlogic.proto



<a name="openmatch.QueryTicketsRequest"></a>

### QueryTicketsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| pool | [Pool](#openmatch.Pool) |  | A Pool is consists of a set of Filters. |






<a name="openmatch.QueryTicketsResponse"></a>

### QueryTicketsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tickets | [Ticket](#openmatch.Ticket) | repeated | Tickets is a list of Ticket representing one or more Tickets which meet all Filter criterias. |





 

 

 


<a name="openmatch.MmLogic"></a>

### MmLogic
The MmLogic service implements helper APIs for Match Function to query Tickets from state storage.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| QueryTickets | [QueryTicketsRequest](#openmatch.QueryTicketsRequest) | [QueryTicketsResponse](#openmatch.QueryTicketsResponse) stream | QueryTickets gets a list of Tickets that match all Filters of the input Pool. - If the Pool contains no Filters, QueryTickets will return all Tickets in the state storage. QueryTickets pages the Tickets by `storage.pool.size` and stream back response. - storage.pool.size is default to 1000 if not set, and has a mininum of 10 and maximum of 10000 |

 



## Scalar Value Types

| .proto Type | Notes | C++ Type | Java Type | Python Type |
| ----------- | ----- | -------- | --------- | ----------- |
| <a name="double" /> double |  | double | double | float |
| <a name="float" /> float |  | float | float | float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long |
| <a name="bool" /> bool |  | bool | boolean | boolean |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str |
