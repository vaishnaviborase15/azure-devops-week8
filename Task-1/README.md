

#  Azure DevOps: Configuring Dashboards and Work Item Queries

This guide helps teams and individuals effectively **configure dashboards and create work item queries** in **Azure DevOps** to visualize progress, monitor work, and drive collaboration.

---

##  Work Item Queries

Azure DevOps Queries allow filtering and retrieving work items based on specific conditions.

###  Query Types
- **Flat List**: Simple list of work items.
- **Tree of Work Items**: Parent-child hierarchy (e.g., Epics > Features > Stories).
- **Direct Links**: Shows work items linked together by specific relationships.

###  Creating a Query
1. Go to **Boards** → **Queries**.
2. Click on **New Query**.
3. Set filters like:
   - `Work Item Type = Bug`
   - `Assigned To = @Me`
   - `Iteration Path = @CurrentIteration`
4. Customize columns and sort order.
5. Save the query for yourself or share it with your team.


##  Dashboards

Azure DevOps Dashboards are customizable pages that display real-time project insights using widgets.

###  Steps to Configure a Dashboard
1. Navigate to your Project → **Overview** → **Dashboards**.
2. Create a new dashboard or edit an existing one.
3. Click **+ Add Widget** to include:
   - **Query Tile**: Display query result count.
   - **Chart for Work Items**: Visual representation of a saved query.
   - **Sprint Burndown**, **Velocity**, **Test Results**, etc.

4. Configure widget settings such as:
   - Linked query
   - Refresh interval
   - Display format (chart type, tile title)

###  Permissions
- Only users with appropriate permissions can **edit or manage** dashboards.
- Widgets respect query-level access restrictions.

---

##  Example Use Case

**Objective**: Visualize all open bugs in the current sprint.

1. Create a **Flat List** query:
   ```txt
   Work Item Type = Bug AND
   State != Closed AND
   Iteration Path = @CurrentIteration
   ```
2. Save it as **Shared Query**.
3. Add:
   - A **Query Tile** widget to show bug count.
   - A **Pie Chart** widget to categorize bugs by severity.

---

##  Benefits
- Enhanced visibility into work progress.
- Real-time team collaboration and tracking.
- Better planning during sprint ceremonies.
- Centralized reporting for stakeholders.

---

##  Resources

-  [Using Queries in Azure DevOps (Microsoft Docs)](https://learn.microsoft.com/en-us/azure/devops/boards/queries/using-queries?view=azure-devops&tabs=browser)
-  [Azure DevOps Dashboards Overview](https://learn.microsoft.com/en-us/azure/devops/report/dashboards/overview?view=azure-devops)
-  [YouTube Tutorial: Azure DevOps Pipelines and Dashboards](https://www.youtube.com/watch?v=xH5EY7FCFQw)


##  Author

**Vaishnavi Borase**  
CSI ID: `CT_CSI_DV_4845`  
Azure Account: `221106014.@rcpit.ac.in`

---
