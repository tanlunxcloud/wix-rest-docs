SortOrder: 0
# About the Resources API

Resources allows you to manage *schedule-able assets* in Wix Bookings. A resource is any business asset that can be booked or assigned to a session. Resources often include staff people, rooms and/or locations, and specialized equipment.
Each resource has its own schedule.

A resource will typically be assigned to an *availability type schedule* (See Schedule documentation for more details). For example, a staff member resource with an *availability type schedule*, will be available for booking based on the free slots on their schedule. The same logic works to manage meeting rooms, based on the room's schedule. 

**Tags system:**  
To identify and group the different type of resources, you can apply a tag to a resource.
For example:
A set of resources with the tag `StaffMember` and another set of resources with the tag `room` be filtered by tag, to provide the data required to display.

We recommend applying tags to resources to categorize staff, rooms, equipment, etc. for easy filtering. 
You can create new tags when creating and updating resources via this API.

## Usage in Wix Bookings

In Wix Bookings, all default resources fall into 2 categories:

- A *Staff Member* resource is a service provider, and has its own schedule by which the staff member is available to provide the service (and therefore, the service is available for booking). A staff member created via the Wix Bookings UI, automatically becomes a reource with the tag `StaffMember`, which cannot be removed.
- A *Business* resource represents the business entity's work schedule. For example, a Yoga Studio and its opening hours. This resource automatically receives the tag `Business` upon creation via the Wix Bookings UI, which cannot be removed.
