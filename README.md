# Satellite Subscriptions Report

Generate an HTML report on your Red Hat subscriptions usage in your Satellite system, on a per-host basis. 

Useful during renewals and for troubleshooting purposes.

## How it works

This Ansible playbook queries the Satellite's *Hosts* API with your filter of choice, and for each host, it queries its data.

The data is then used to compile the HTML report. For each playbook run with a different query, the results are added to the final report.

## How to use it

The best use for this playbook is to be run from the Satellite server itself. It does not make use of the Foreman Ansible modules.

Use the [satellite-vars.yml.example] file to create your own `satellite-vars.yml` file. You should set your Satellite's URL and credentials with read access to hosts and subscriptions (i.e. the *Viewer* role).

Run the playbook with your query of choice:

`$ ansible-playbook main.yml -e satellite_hosts_search_query="' os_title \!~ centos or hypervisor = true '"

*(based on your Bash version, you might or might not need that extra backslash before the exclamation mark)*

You can run the playbook multiple times with different queries, and since the previous query results are cached, the results are *added* to the new report.

To clean up the cache, simply remove all `.json` files from the *cache* directory.

## The Report

The report is a *left join* between the Hosts list and its subscriptions. It means, for each Host, it is shown multiple times - once for each subscription attached - and once if no subscriptions are attached.

Each hyperlink brings you to a handy page where to apply modifications, i.e.:
- A host's FQDN with no subscriptions: to the Add Subscriptions page
- A host's FQDN with subscriptions: to the List/Remove Subscriptions page
- A Subscription's name: to the Subscription Info page

