# Self-Service IT Request Portal Project

## Project Overview
Create a self-service portal in ServiceNow that allows employees to request IT services such as software installation, hardware requests, or password resets. The portal will include a user-friendly interface, automated workflows, and basic reporting to track request status. This project demonstrates skills in ServiceNow’s Service Catalog, Workflow Designer, Client Scripts, and Business Rules.

## Objectives
- Build a Service Catalog with request items for common IT services.
- Implement automated workflows to route requests to the appropriate IT team.
- Create a simple dashboard to track request status and resolution times.
- Use scripting to add validations and enhance user experience.
- Ensure the portal is intuitive and aligns with ITIL best practices.

## Tools and Technologies
- **ServiceNow Platform**: Use a Personal Developer Instance (PDI) from developer.servicenow.com.
- **Modules**: Service Catalog, Workflow Designer, Flow Designer, Script Includes, Client Scripts, Business Rules.
- **Languages**: JavaScript, Glide API, HTML/CSS (for UI customization).
- **Optional**: Service Portal (for a modern front-end experience).

## Project Steps

### 1. Setup and Planning
- **Get a PDI**: Sign up for a free Personal Developer Instance at [developer.servicenow.com](https://developer.servicenow.com).
- **Define Scope**:
  - Catalog Items: Software Installation, Hardware Request, Password Reset.
  - Workflow: Request submission → Approval (if needed) → Fulfillment → Closure.
  - Users: Employees (requesters), IT team (fulfillers).
- **Create Tables** (if needed):
  - Extend the `sc_request` table for custom fields like "Urgency" or "Device Type."
  - Use out-of-the-box tables for simplicity where possible.

### 2. Build the Service Catalog
- **Create Catalog Items**:
  - Navigate to **Service Catalog > Catalog Definitions > Maintain Items**.
  - Add three items: "Software Installation," "Hardware Request," "Password Reset."
  - For each item:
    - Add variables (e.g., Software Name, Hardware Type, Urgency).
    - Set a basic price (optional, for realism).
    - Assign to the "IT Service Catalog" category.
- **Configure Forms**:
  - Use Form Designer to customize the request form layout.
  - Add UI Policies to show/hide fields based on user input (e.g., show "Software Version" only if "Software Installation" is selected).

### 3. Design Workflows
- **Using Flow Designer**:
  - Navigate to **Flow Designer** and create a new flow for each catalog item.
  - Example Flow for "Software Installation":
    1. Trigger: New catalog request submitted.
    2. Action: Check if approval is needed (e.g., based on cost or software type).
    3. Action: Assign to IT group (e.g., "Software Support Team").
    4. Action: Send notification to the requester on status updates.
    5. Action: Close request upon fulfillment.
- **Alternative**: Use Workflow Editor for more complex logic if preferred.
- **Automate Notifications**:
  - Configure email notifications for request submission, approval, and completion.

### 4. Add Scripting for Functionality
- **Client Script** (for real-time validation):
  - Create an onChange Client Script for the "Software Name" variable.
  - Example: Prevent users from requesting unlicensed software.
    ```javascript
    function onChange(control, oldValue, newValue, isLoading) {
        if (isLoading || newValue == '') {
            return;
        }
        var restrictedSoftware = ['Adobe Photoshop', 'AutoCAD']; // Example list
        if (restrictedSoftware.indexOf(newValue) != -1) {
            g_form.setValue('software_name', '');
            alert('This software requires special approval. Please contact IT.');
        }
    }
    ```
- **Business Rule** (for backend logic):
  - Create a Business Rule to auto-populate the "Assigned To" field based on the request type.
    ```javascript
    (function executeRule(current, previous) {
        if (current.cat_item.name == 'Software Installation') {
            current.assigned_to = 'sys_id_of_software_team_member'; // Replace with actual sys_id
        }
    })(current, previous);
    ```
- **Script Include** (for reusable logic):
  - Create a Script Include to validate hardware availability.
    ```javascript
    var HardwareCheck = Class.create();
    HardwareCheck.prototype = {
        initialize: function() {},
        isAvailable: function(hardwareType) {
            var gr = new GlideRecord('cmdb_ci_hardware');
            gr.addQuery('type', hardwareType);
            gr.addQuery('install_status', 'In Stock');
            gr.query();
            return gr.hasNext();
        },
        type: 'HardwareCheck'
    };
    ```

### 5. Create a Service Portal (Optional)
- **Customize the Portal**:
  - Navigate to **Service Portal > Portals** and use the default portal or create a new one.
  - Add a widget to display the Service Catalog items.
  - Use HTML/CSS to style the portal for a modern look.
- **Example Widget**:
  - Create a widget to show "My Requests" with status and details.
  - Use AngularJS to fetch and display user requests dynamically.

### 6. Build a Dashboard
- **Create a Report**:
  - Navigate to **Reports > Create New**.
  - Create a bar chart showing "Requests by Status" (Open, In Progress, Closed).
  - Create a line chart for "Average Resolution Time" by request type.
- **Add to Dashboard**:
  - Navigate to **Performance Analytics > Dashboards**.
  - Create a new dashboard named "IT Request Metrics."
  - Add the reports as widgets.

### 7. Testing and Validation
- **Test Cases**:
  - Submit requests as an employee and verify workflow execution.
  - Test approval processes (if applicable).
  - Validate notifications and dashboard accuracy.
- **Debug Scripts**:
  - Use the Script Debugger to troubleshoot Client Scripts and Business Rules.
  - Check System Logs for errors.

### 8. Documentation and Presentation
- **Document the Project**:
  - Create a README file summarizing:
    - Project purpose and features.
    - Setup instructions for the PDI.
    - Screenshots of the portal, workflows, and dashboard.
  - Include code snippets for key scripts.
- **Portfolio Presentation**:
  - Record a 5-minute demo video walking through the portal, submitting a request, and showing the dashboard.
  - Highlight challenges faced and how you solved them (e.g., scripting issues).

## Deliverables
- A functional Self-Service IT Request Portal in your PDI.
- At least three catalog items with workflows and notifications.
- Client Scripts, Business Rules, and a Script Include for custom logic.
- A dashboard with two reports.
- Documentation and a demo video for your portfolio.

## Learning Outcomes
- Proficiency in ServiceNow Service Catalog and Workflow Designer.
- Hands-on experience with JavaScript, Glide API, and UI customization.
- Understanding of ITIL processes (Request Fulfillment).
- Ability to create dashboards and reports for stakeholders.
- Practical experience in testing and debugging ServiceNow applications.

## Tips for Success
- Start small: Focus on one catalog item before scaling to others.
- Use ServiceNow’s documentation and community forums for guidance.
- Experiment in your PDI without fear of breaking things.
- Align with ITSM best practices to make the project realistic.
- Showcase this in your resume under "Projects" with a link to your GitHub README or demo video.

## Resources
- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [Now Learning: Application Development Fundamentals](https://learning.servicenow.com)
- [ServiceNow Community](https://community.servicenow.com)
- YouTube tutorials (search for “ServiceNow Service Catalog Tutorial”)
