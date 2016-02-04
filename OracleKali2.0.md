**How to Setup Oracle in Kali 2.0** 

I recently have started preparation
for an exam which requires the use of Kali for testing and noticed that
out of the box Kali 2.0 Sana or rolling do not support oracle. So I set
about to find out how to remediate this and fix. There are guides out
there on how to fix for Kali 1.x but none for Kali 2, at the time of
writing. Now you'd think the process would be the same however it's not
as simple as you'd think. When you try to use a module within metasploit
such as oracle\_login, metasploit gives you an error similar to this:

    use auxiliary/admin/oracle/oracle_login
    set RHOST 127.0.0.1
    msf auxiliary(oracle_login)> run [-] Failed to load the OCI library: cannot load such file -- oci8 [-] Try 'gem install ruby-oci8' [*] Auxiliary module execution completed
    msf auxiliary(oracle_login)>

I found that when running the recommended `gem install ruby-oci8` did
not work however your mileage may vary, if it doesn't work for you then
follow this guide. First you will need to create an account on
oracle.com in order to download the necessary files, don’t worry, a temp
email works just fine as you don't need to confirm it in order to
download necessary files. 

**Setup Directories & Acquire Relevant Files**


First you'll need to create relevant directories to get things setup,
create the directory `/opt/oracle` and then `cd` into it. Next you'll
need to download the Oracle Instant Client files. Depending if you are
using 32-bit or 64-bit you'll need different files that can be found
[here(32)](http://www.oracle.com/technetwork/topics/linuxsoft-082809.html)
and
[here(64)](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html).
From these links regardless of your architecture you'll need to get the
following files:

-   instantclient-basic-linux-12.1.0.2.0.zip
-   instantclient-sqlplus-linux-12.1.0.2.0.zip
-   instantclient-sdk-linux-12.1.0.2.0.zip

Once downloaded you'll want to move them from `~/Downloads` to
`/opt/oracle`, using command
`mv ~/Downloads/instantclient-*.zip /opt/oracle`, then proceed to unzip
them within /opt/oracle this should look similar to output below:

     
    root@kali:/opt/oracle# unzip instantclient-basic-linux.x64-12.1.0.2.0.zip instantclient-sdk-linux.x64-12.1.0.2.0.zip instantclient-sqlplus-linux.x64-12.1.0.2.0.zip 
    Archive:  instantclient-basic-linux.x64-12.1.0.2.0.zip 
      inflating: instantclient_12_1/adrci  
      inflating: instantclient_12_1/BASIC_README  
      inflating: instantclient_12_1/genezi  
      inflating: instantclient_12_1/libclntshcore.so.12.1  
      inflating: instantclient_12_1/libclntsh.so.12.1  
      ----SNIP----

**Changing file permissions and relevant paths**

Once unzipped you should now have an instantclient\_12\_1 directory, this is all the files
required to get the `sqlplus` client working. In order to integrate with
metasploit we also need to get some things linked up and additional to
this ruby needs to be modified too. First you'll need to link a .so file
as shown:

    root@Pentest-Kali-AG:/opt/oracle/instantclient_12_1# ln libclntsh.so.12.1 libclntsh.so
    root@kali:/opt/oracle/instantclient_12_1# ls -lh libclntsh.so
    -rwxrwxr-x 2 root root 57M Jul  7  2014 libclntsh.so

After this is complete we'll also need to make sure our bash environment
is *oracle enabled*, in order to do this we need to setup some
environment variables. The easiest way to do this is to append to either
`.bashrc` or `/etc/bash_bashrc`.

    root@kali:/opt/oracle/instantclient_12_1# echo "
    # ORACLE
    export PATH=$PATH:/opt/oracle/instantclient_12_1
    export SQLPATH=/opt/oracle/instantclient_12_1
    export TNS_ADMIN=/opt/oracle/instantclient_12_1
    export LD_LIBRARY_PATH=/opt/oracle/instantclient_12_1
    export ORACLE_HOME=/opt/oracle/instantclient_12_1
    " >> ~/.bashrc
    root@kali:/opt/oracle/instantclient_12_1# tail ~/.bashrc
    # ORACLE
    export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/oracle/instantclient_12_1
    export SQLPATH=/opt/oracle/instantclient_12_1
    export TNS_ADMIN=/opt/oracle/instantclient_12_1
    export LD_LIBRARY_PATH=/opt/oracle/instantclient_12_1
    export ORACLE_HOME=/opt/oracle/instantclient_12_1

Now that is done, we need to load bashrc as our current working from
file, this can be done with the source command, next is to check that
these settings have been loaded, this can been seen:

    root@kali:/opt/oracle/instantclient_12_1# source ~/.bashrc
    root@kali:/opt/oracle/instantclient_12_1# env | grep oracle
    OLDPWD=/opt/oracle
    LD_LIBRARY_PATH=/opt/oracle/instantclient_12_1
    TNS_ADMIN=/opt/oracle/instantclient_12_1
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/oracle/instantclient_12_1
    PWD=/opt/oracle/instantclient_12_1
    SQLPATH=/opt/oracle/instantclient_12_1
    ORACLE_HOME=/opt/oracle/instantclient_12_1

Optionally now you can reboot to double check these settings have been
successfully applied. **Setting up Metasploit** The final stage in
getting this all working is making metasploit happy, this step requires
[ruby-oci8](https://github.com/kubo/ruby-oci8/archive/ruby-oci8-2.1.7.zip)
to be downloaded. Navigate back to `/opt/oracle` and download it:

    # wget https://github.com/kubo/ruby-oci8/archive/ruby-oci8-2.1.7.zip
    --2016-02-04 17:57:35--  https://github.com/kubo/ruby-oci8/archive/ruby-oci8-2.1.7.zip
    ----SNIP-----
    Saving to: ‘ruby-oci8-2.1.7.zip’
    ----SNIP-----
    2016-02-04 17:57:36 (594 KB/s) - ‘ruby-oci8-2.1.7.zip’ saved [278270/278270]
    root@kali:/opt/oracle# unzip ruby-oci8-2.1.7.zip
    Archive:  ruby-oci8-2.1.7.zip
    fb913e32d8a09bd46e5bf549bd8e554f0870d384
       creating: ruby-oci8-ruby-oci8-2.1.7/
      inflating: ruby-oci8-ruby-oci8-2.1.7/.gitignore
    ----SNIP-----
      inflating: ruby-oci8-ruby-oci8-2.1.7/test/test_package_type.rb  
      inflating: ruby-oci8-ruby-oci8-2.1.7/test/test_rowid.rb  

Then cd into the directory that was created `ruby-oci8-ruby-oci8-2.1.7`
and install the `ruby-dev` and `libgmp-dev` packages if you haven't
already:

    root@kali:/opt/oracle# cd ruby-oci8-ruby-oci8-2.1.7/
    root@kali:/opt/oracle/ruby-oci8-ruby-oci8-2.1.7# apt-get install ruby-dev libgmp-dev

Make sure that the ruby interpreter that you will be use is the same as
what Metasploit is using: `export PATH=/opt/metasploit/ruby/bin:$PATH`
Finally we need to `make && make install`. logout or reboot and then
test that the tools are operational.

    msf > use auxiliary/admin/oracle/oracle_login
    msf auxiliary(oracle_login) > set RHOST 127.0.0.1
    RHOST => 127.0.0.1
    msf auxiliary(oracle_login) > run

    [*] Starting brute force on 127.0.0.1:1521...
    [*] Auxiliary module execution completed
    msf auxiliary(oracle_login) > 

Success! All fixed :-D
