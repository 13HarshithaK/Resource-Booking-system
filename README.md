# Resource-Booking-system
A WordPress-based booking system for appointments and resources, featuring a calendar view of scheduled timeslots and a billing module that generates invoices based on usage or time booked. Built as a working prototype for a university contract.

# Setup needed to run this code:

The main booking system can be any plugin that has a free tier and handles the scheduling of appointments. For this prototype, the Amelia plugin was used. 

To quickly have this prototype running, use Local to simulate the wordpress environment. 
<img width="959" height="437" alt="image" src="https://github.com/user-attachments/assets/e0277e3b-2713-46d8-bd44-70f2593f4dc5" />

Setup the desired plugin, configure the settings as instructed for the plugin in their tutorials, for this prototype only 2 resources were considered:
<img width="854" height="231" alt="image" src="https://github.com/user-attachments/assets/ace80bce-9825-494a-a2c1-c3d9fbc33c99" />

Then use code snippets plugin to create 2 new pages, one for the calendar view and one for the billing module:

<img width="808" height="355" alt="image" src="https://github.com/user-attachments/assets/6edbef07-5785-4fe6-a89a-48ad8b728afa" />

In these files, copy paste the code from the respective text files. Hit save and activate to create the page:

<img width="947" height="356" alt="image" src="https://github.com/user-attachments/assets/83e25890-6317-46a6-997f-187bed7e708f" />

Use the created shortcodes to call on these modules and render the pages:

<img width="959" height="427" alt="image" src="https://github.com/user-attachments/assets/b949d1a0-1c7c-4bf6-ae70-0f6fdebb6ad0" />

The calendar view looks like this:

<img width="641" height="418" alt="image" src="https://github.com/user-attachments/assets/34e19a02-f06b-49b8-b2e1-2a7e810adeff" />

You can switch to the weekly view by clicking the button 'Weekly view', it will show the current week by default. Clicking on a specific day in the monthly view will display the data for that specific week instead:

<img width="638" height="388" alt="image" src="https://github.com/user-attachments/assets/e113fd45-9f3b-4797-b042-5db650b9f8dd" />

The billing dashboard looks like this:

<img width="633" height="320" alt="image" src="https://github.com/user-attachments/assets/4136212b-b4bd-43a2-abd4-ca9ea04355c3" />

Selecting a person gives their usage/booking history for the month. You can generate the invoce using the blue button:

<img width="464" height="404" alt="image" src="https://github.com/user-attachments/assets/af92d829-045a-4b44-8b3a-c5796db94fa6" />

It generates an output like this which can be saved as a pdf:

<img width="205" height="388" alt="image" src="https://github.com/user-attachments/assets/1c442587-5535-413f-8220-8a1670121867" />

You can also opt for a weekly usage breakdown and generate the corresponding invoice:

<img width="479" height="418" alt="image" src="https://github.com/user-attachments/assets/345fb1bd-ff62-4fa4-a3b9-000bd54a9c6c" />

# Notes:
This system uses the data that the booking plugin stores in the wp databases. If you wish to use a different plugin, most of the code remains unchanged, just switch out these lines to point to the correct databases:

**Calendar View (calendar_view_php.txt), Monthly view — lines 231–243:**

FROM {$wpdb->prefix}amelia_appointments a

LEFT JOIN {$wpdb->prefix}amelia_customer_bookings cb ON a.id = cb.appointmentId

LEFT JOIN {$wpdb->prefix}amelia_users u ON cb.customerId = u.id


**Calendar View (calendar_view_php.txt), Weekly view — lines 357–369: (same structure, different date range):**

FROM {$wpdb->prefix}amelia_appointments a

LEFT JOIN {$wpdb->prefix}amelia_customer_bookings cb ON a.id = cb.appointmentId

LEFT JOIN {$wpdb->prefix}amelia_users u ON cb.customerId = u.id


**Billing Module (Billing_module_php.txt), Customer dropdown — lines 26–31:**

FROM {$wpdb->prefix}amelia_users u

INNER JOIN {$wpdb->prefix}amelia_customer_bookings cb ON u.id = cb.customerId


**Billing Module (Billing_module_php.txt), Billing data — lines 578–592:**

FROM {$wpdb->prefix}amelia_appointments a

INNER JOIN {$wpdb->prefix}amelia_customer_bookings cb ON a.id = cb.appointmentId

LEFT JOIN {$wpdb->prefix}amelia_services s ON a.serviceId = s.id


**Finally, Column names pulled from these tables (bookingStart, bookingEnd, serviceId, customerId, firstName, lastName, price, name) are also Amelia-specific and may differ in other plugins.**
