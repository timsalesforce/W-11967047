# W-11967047 Lightning Console - Parent Campaign field shows blank value after hover on inaccessible record

## Repro steps
1. Deploy code to a scratch org
2. Create a new User and assign the Profile - Standard User No Account
3. As Sys Admin, create a Campaign and a child Campaign
4. Change ownership of the child campaign to the Standard User created in #2
5. In Sharing Settings, make Campaigns Private for internal users
6. In General Sharing Settigns, uncheck "Require permission to view record names in lookup fields"
7. Login as the Standard User
8. Switch to the Lightning Console App
9. Open the child Campaign
10. Hover over the Parent Campaign field (you should see the name and link, but will not have read access to it)
11. Observe an error message pops up
12. Close the child campaign workspace tab
13. Re-open the child campaign
14. Observe the Parent Campaign field is now blank

This is happening because ADS is evicting the parent campaign when the getRecord fails.  That happens here:
https://sourcegraph.soma.salesforce.com/perforce.soma.salesforce.com/app/main/core/-/blob/ui-force-components/components/force/recordLibrary/records.js?L700:29&popover=pinned

The request is to check the "Require permission to view record names in lookup fields" sharing pref before evicting it.

This will require a TD for the RAX team which will need to expose the pref to the UIAPI.
