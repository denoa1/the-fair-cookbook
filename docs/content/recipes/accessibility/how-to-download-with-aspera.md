---
recipe_metadata:
  - identifier: "none"
  - version: 1.0
difficulty_level: 3
reading_time_in_minutes: 20
type_of_document: "hands on, step-by-step recipe"
document_contains_executable_code: false
intended_audience:
  - "data scientists"
---


# How to download files with Aspera

## Abstract

A recipe to download files from an Aspera Site, it will also help with the uploading. Providing some guidance on how to do this.
This is part of  a group of other related recipes such as the ftp upload/download to help us all more efficiently get to be uploading and downloading files .

## Graphical overview of the Recipe

[![](https://mermaid.ink/img/eyJjb2RlIjoiZ3JhcGggTFI7XG5cbiBBW0RhdGEgb24gc291cmNlIHJlcG9zaXRvcnldIC0tPkIoRGVjaWRlIGluIG5lZWRlZCBsb2NhdGlvbilcbiAgICBCIC0tPiBDe0RvIHlvdSBoYXZlIGFuIGFjY291bnQgb24gc291cmNlIHJlcG9zaXRvcnk_fVxuICAgIEMgLS0-fFllc3wgRFtEZWNpZGUgaG93IHlvdSBhcmUgeW91IGdvaW5nIGFjY2VzcyB0aGUgZGF0YV1cbiAgICBDIC0tPnxOb3wgRVtHZXQgYW4gYWNjb3VudCBpZiB5b3UgYXJlIHRoZSBhcHBsaWNhYmxlIHBlcnNvbl1cbiAgICBEIC0tPkh7SW5zdGFsbCBkb3dubG9hZC91cGxvYWQgc29mdHdhcmV9XG4gICAgSCAtLT5Je1dvcmsgb3V0IHRoZSBhcHByb3ByaWF0ZSBjb21tYW5kIGxpbmUgb3B0aW9uc31cbiAgICBJIC0tPlp7RG9lcyB5b3VyIHN5c3RlbSBoYXZlIHRoZSBhcHBsaWNhYmxlIGZpcmV3YWxsIGhvbGVzP31cbiAgICBaIC0tPnxZZXN8IEZbSGF2ZSB0aGUgZmlyZXdhbGwgaG9sZXNdXG4gICAgWiAtLT58Tm98IEdbR2V0IGZpcmV3YWxsIGhvbGVzXVxuICAgIEYgLS0-SntTdGFydCBkb3dubG9hZH1cbiAgICBKIC0tPkt7TW9uaXRvciBkb3dubG9hZCBhbmQgY2hlY2sgZG93bmxvYWQgc3BlZWR9XG4gICAgSyAtLT5Me0NoZWNrIGRvd25sb2FkIGFuZCBjcmVhdGUgYSBtaW5pIGRhdGEgY2F0YWxvZ3VlfVxuXG4iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9LCJ1cGRhdGVFZGl0b3IiOmZhbHNlfQ)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggTFI7XG5cbiBBW0RhdGEgb24gc291cmNlIHJlcG9zaXRvcnldIC0tPkIoRGVjaWRlIGluIG5lZWRlZCBsb2NhdGlvbilcbiAgICBCIC0tPiBDe0RvIHlvdSBoYXZlIGFuIGFjY291bnQgb24gc291cmNlIHJlcG9zaXRvcnk_fVxuICAgIEMgLS0-fFllc3wgRFtEZWNpZGUgaG93IHlvdSBhcmUgeW91IGdvaW5nIGFjY2VzcyB0aGUgZGF0YV1cbiAgICBDIC0tPnxOb3wgRVtHZXQgYW4gYWNjb3VudCBpZiB5b3UgYXJlIHRoZSBhcHBsaWNhYmxlIHBlcnNvbl1cbiAgICBEIC0tPkh7SW5zdGFsbCBkb3dubG9hZC91cGxvYWQgc29mdHdhcmV9XG4gICAgSCAtLT5Je1dvcmsgb3V0IHRoZSBhcHByb3ByaWF0ZSBjb21tYW5kIGxpbmUgb3B0aW9uc31cbiAgICBJIC0tPlp7RG9lcyB5b3VyIHN5c3RlbSBoYXZlIHRoZSBhcHBsaWNhYmxlIGZpcmV3YWxsIGhvbGVzP31cbiAgICBaIC0tPnxZZXN8IEZbSGF2ZSB0aGUgZmlyZXdhbGwgaG9sZXNdXG4gICAgWiAtLT58Tm98IEdbR2V0IGZpcmV3YWxsIGhvbGVzXVxuICAgIEYgLS0-SntTdGFydCBkb3dubG9hZH1cbiAgICBKIC0tPkt7TW9uaXRvciBkb3dubG9hZCBhbmQgY2hlY2sgZG93bmxvYWQgc3BlZWR9XG4gICAgSyAtLT5Me0NoZWNrIGRvd25sb2FkIGFuZCBjcmVhdGUgYSBtaW5pIGRhdGEgY2F0YWxvZ3VlfVxuXG4iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9LCJ1cGRhdGVFZGl0b3IiOmZhbHNlfQ)

<div class="mermaid">
graph LR;

 A[Data on source repository] -->B(Decide in needed location)
    B --> C{Do you have an account on source repository?}
    C -->|Yes| D[Decide how you are you going access the data]
    C -->|No| E[Get an account if you are the applicable person]
    D -->H{Install download/upload software}
    H -->I{Work out the appropriate command line options}
    I -->Z{Does your system have the applicable firewall holes?}
    Z -->|Yes| F[Have the firewall holes]
    Z -->|No| G[Get firewall holes]
    F -->J{Start download}
    J -->K{Monitor download and check download speed}
    K -->L{Check download and create a mini data catalogue}
</div>


## 9.Authors:

| Name           | Affiliation    | orcid          | CrediT role   |
| :------------- | :------------- | :------------- |:------------- |
| Peter Woolllard  | GSK, metadata group in R&D Data and Computational Sciences  |  0000-0002-7654-6902 |  Writing - Original Draft |
