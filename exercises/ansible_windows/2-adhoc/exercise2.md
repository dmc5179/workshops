Table of Contents
=================

-   [Agenda](index.html)

-   [Exercise 1 - Configuring Ansible Tower](exercise1.html)

-   [Exercise 2 - Ad-hoc commands in Tower](exercise2.html)

-   [Exercise 3 - Writing Your First playbook](exercise3.html)

-   [Exercise 4 - Creating and Running a Job Template](exercise4.html)

-   [Exercise 5 - Using Variables, Loops, and Handlers](exercise5.html)

-   [Exercise 6 - Roles: Making your playbooks reusable](exercise6.html)

-   [Exercise 7 - Using Ansible for Windows Patching](exercise7.html)

-   [Wrap UP](wrapup.html)

Section 1: Ad-hoc commands
==========================

For our first exercise, we are going to run some ad-hoc commands to help
you get a feel for how Ansible works. Ansible Ad-Hoc commands enable you
to perform tasks on remote nodes without having to write a playbook.
They are very useful when you simply need to do one or two things
quickly and often, to many remote nodes.

Step 1:
-------

To start, we will need to go to our Inventory. So click **Inventories**
on the left panel, and then click the name of our Inventory **Ansible
Workshop Inventory**. Now that you are on the Inventory Details page, we
will need to go select our Host. So click **HOSTS**.

Next to each host is a checkbox. Check the box next to each host you
want to run an Ad-Hoc Command on. You will then see the **RUN COMMANDS**
button become enabled. Click it now.

![Run Command](images/2-adhoc-run-command.png)

This will pop up the **Execute Command** window. From here is where we
can run a single task against our hosts.

Let’s start with something really basic - pinging a host. The `win_ping`
module makes sure our windows hosts are responsive. This is not a
traditional *ping*, but actually verifying both connectivity and
authentication to the host.

Fill out this form as follows

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_ping</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>LIMIT</p></td>
<td><p>(This will be pre-filled out for you with the hosts you selected)</p></td>
</tr>
<tr class="even">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

![Run Win\_Ping](images/2-adhoc-run-win_ping.png)

Once you click **LAUNCH** you will be redirected to the Job log. Every
job and action in Ansible Tower is recorded and stored. These logs can
be auto-rotated and can also be exported automatically to another
logging system such as Splunk or ELK.

The first part of the log shows you the details of the job. This
includes information such as who launched the job, against what hosts,
and when.

![Win\_Ping Log Details](images/2-adhoc-run-win_ping-success.png)

The 2nd part of the job log shows you the actual output from the
command. If your connection was successful, you should see a result such
as this.

![Win\_Ping Log Details](images/2-adhoc-run-win_ping-output.png)

The results returned will be different depending on which module is
used, as they all handle and deal with different data sets depending on
the task. No matter which module is used, you will always see a color
coded STATUS of either SUCCESS, FAILURE, CHANGED, or SKIPPING.

Step 2:
-------

Now let’s see how we can run a PowerShell command and view the output
using the `win_shell` module.

Let’s fill out the form again, but this time use the `win_shell` module
to execute the `Get-Service` Powershell command.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_shell</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td><p>Get-Service</p></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

Launch the job and view the results. You will see that it returns a
direct output of what the Powershell command returned. This data can be
stored to a variable and directly parsed inside your Ansible playbook
later on.

And run it one more time with the `Get-Process` Powershell command.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_shell</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td><p>Get-Process</p></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

Step 3:
-------

We will now take a look at your Windows nodes configuration. The `setup`
module queries the remote host for various data and returns that data as
Ansible Facts. This data is useful to determine things such as OS
Versions, Hardware Configuration, and other various data points. This
can then be used in your playbook for all sorts of reasons such as
determining whether a task should even run, or determining what the name
of a package should be based upon the OS Version.

The `setup` module will run automatically at the beginning of every
playbook unless told not to. That way this data is always available to
your playbook.

Lets go ahead and run the `setup` module to look at the output. Fill out
the **EXECUTE COMMAND** form again with this info.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>setup</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

You will then see results like this

![Setup Log Details](images/2-adhoc-run-setup-output.png)

Step 4:
-------

Now, let’s install IIS using the `win_feature` module. Our arguments
parameter is going to get a little more complex now.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_feature</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td><p>name=Web-Server state=present</p></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

You will notice that the log text is now orange. This is to denote that
a change was made on the system versus the green that shows that no
changes were made earlier.

![Win\_Feature Log Details](images/2-adhoc-run-win_feature-output.png)

Step 5:
-------

OK, IIS is installed now so let’s be certain it is started using the
`service` module.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_service</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td><p>name=W3Svc state=started</p></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

Step 6:
-------

Finally, let’s clean up after ourselves. First, stop the IIS service.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_service</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td><p>name=W3Svc state=stopped</p></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

Step 7:
-------

Next, remove the IIS feature.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_feature</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td><p>name=Web-Server state=absent</p></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

And now reboot the host.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>MODULE</p></td>
<td><p>win_reboot</p></td>
</tr>
<tr class="even">
<td><p>ARGUMENTS</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>MACHINE CREDENTIAL</p></td>
<td><p>Student Account</p></td>
</tr>
</tbody>
</table>

> **Note**
>
> The win\_reboot module will cause the machine to reboot, and then will
> wait for it to come completely back up before finishing. This way, if
> you need to reboot the host in the middle of your playbook, the rest
> of the playbook will not fail because the host is inaccessible.
