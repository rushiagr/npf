+++
title = "A different approach to AWS CLI"
date = "2016-01-02T00:00:00-00:00"
tags = ["commandline", "cli", "terminal", "ux", "aws", "python", "boto"]
type = "post"
+++


For a user of Amazon Web Services, quickly viewing the dashboard of all
resources is painful. If you use a web browser, your session won't persist
for more than a day and you will have to re-authenticate every day. If you use
AWS CLI, then you might not remember the exact command ('was it
`aws ec2 describe-key-pair` or `aws ec2 describe-keypair` or
`aws ec2 describe-keypairs`?) but even if you do, the output is a JSON. For viewing
virtual machines, you might not need to see hundreds of lines of details of
your virtual machiens. Maybe you just wanted to see the IP of the VM so that
you can SSH into it. And maybe you just wanted to quickly create a virtual
machine to test something: do you remember all
the parameters you need to specify for instance creation? And don't forget that
you will need to specify the AMI ID of the image, even if you exactly know
which distribution and version of operating system you want the virtual machine
to have.

There is no doubt that AWS CLI is a phenomenal piece of work. It allows you to
do absolutely everything you can do with Amazon's cloud offering. The
documentation is thorough and it is great for automation too, as the only
thing one needs to do is to parse the JSON output. However, it's not as
human-friendly. It doesn't tell you which options are mandatory and which are
optional. It doesn't remind you if you forget to specify the keypair for an
instance which generally results in Google-searching 'aws cli delete instance'
and creating another instance. The CLI has some inconsistencies too -- creating
a keypair is `aws ec2 create-key-pair` but creating a virtual machine is not
`aws ec2 create-instance` but `aws ec2 run-instances`.

To alleviate these pains, I created a simple CLI tool called "[CCH - Cloud CLI
for Humans](http://github.com/rushiagr/aclih)". Just typing `lsvm` prints all
the virtual machines you have in the cluster. Each command can be run without
passing a parameter to it. If an operation requires additional parameters and
you didn't specify it for the first time, it'll ask you to input those
parameters. For example `mkvm`, the command to create virtual machines:

    r@rushi:~$ mkvm
    Available flavors: t2.micro, t2.nano, ...
    Select flavor ['l' to list]:

Commands for resource creation are short and consistent (`mkvm` creates virtual
machines, `mkkp` creates keypairs, `mksg` create security groups). No need to
remember AMI IDs (presently it selects Ubuntu 16.04 64-bit image by default for
you, but in future you might specify an OS name and OS version). All the
commands supported so far are:

    lsvm    - List all virtual machines
    mkvm    - Create a virtual machine
    stpvm   - Stop a virtual machine
    rmvm    - Terminate a virtual machine

    lskp    - List all keypairs
    mkkp    - Create keypairs
    rmkp    - Delete a keypair

    lssg    - List all security groups (including a detailed view)
    mksg    - Create a security group (including specifying secgroup rules)
    rmsg    - Delete a security group

## Installation

I have added the tool to PyPI, so download is easy:

    pip install cch

Run `aws configure` if you don't have AWS credentials configured on your
system. Typically, credentials are kept in `~/.aws/credentials` file.

## Sample usage

For full list of operations supported so far, see this [asciinema
screencast](https://asciinema.org/a/ektm98481nniu7rldc1ncu5af) I'm providing
examples here for some of the commands.

See help text of a command

    r@rushi:~$ lsvm -h
    lsvm [-h] [-s] [<name>]
        -h      Prints helptext and exits
        -s      Prints sizes of VM disks in GB, starting with root disk
        <name>  Only prints VM whose name contains '<name>'

List all virtual machines

    r@rushi:~$ lsvm
        ID              Name           Status   Flavor        IP      Vols
    i-abcd1234     rushi dev m/c      running  t2.micro 52.12.123.123  1
    i-abcd1233   rushi pkg builder    running  t2.micro 52.12.123.122  1
    i-abcd1232 rushi vanilla devstack running  t2.large 54.12.123.121  1
    i-abcd1231  rushi dbaas devstack  running m4.xlarge 52.12.123.120  1


Also show sizes of volumes of instances:

    r@rushi:~$ lsvm  -s
        ID                   Name               Status   Flavor        IP       Vols(GB)
    i-abcd1234          rushi dev m/c          running  t2.micro 52.12.123.123    [8]
    i-abcd1233        rushi pkg builder        running  t2.micro 52.12.123.122    [8]
    i-abcd1232      rushi vanilla devstack     running  t2.large 54.12.123.121    [50]
    i-abcd1231       rushi dbaas devstack      running m4.xlarge 52.12.123.120    [50]

List all VMs whose name contains word 'devstack'

    r@rushi:~$ lsvm devstack
        ID              Name           Status   Flavor        IP      Vols
    i-abcd1232 rushi vanilla devstack running  t2.large 54.12.123.121  1
    i-abcd1231  rushi dbaas devstack  running m4.xlarge 52.12.123.120  1

Create a virtual machine

    r@rushi:~$ mkvm
    Only Ubuntu image supported as of now
    Available flavors: ['t1.micro', 'm1.small', ... ]
    Select flavor ['l' to list]: t2.micro
    Available key pairs: ['rushi-kp-1', 'prod-keypair', 'test-keypair']
    Select keypair: rushi-kp-1
    Available security groups: ['Rushi SecGroup', 'openToAll', 'Rushi SG', 'Rushi Devstack SG']
    Select security group. None to create new one: Rushi SecGroup
    Enter root volume size in GBs: 8
    r@rushi:~$

Any feedback is welcome. Create a PR or issue, or comment below!

Thank you :)
