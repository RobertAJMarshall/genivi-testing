# genivi-testing
Test scripts

Scripts to enable the automatic acceptance testing of QEMU genivi images.

There are 2 user invocable files runOneTest.py and runAllTests.py

runOneTest.py expects a test name to be supplied, currently only a python
file (without the .py suffix), see the example files in Tests as an indication
of how to derive new ones. The class in each test file should inherit from geniviTest.TestGeniviQemu
The test method must have a name with a test_ prefix. More than one test file can
be supplied on the command line

runAllTests.py runs all the tests it finds in Tests as well as a set of tests in coreTests.py

The infrastructure handles booting the QEMU image. It is assumed that
the tests are run from the gdp-src-build directory though if you set
the QEMU_IMAGE_DIR environment variable you can test on images
elsewhere - or ones you have downloaded from the GENIVI download area.
The test infrastructure looks for a bzImage file in the appropriate locations and a file
system in genivi-dev-platform-qemux86-64.ext4
The image shuts down automatically after all tests have been run.

It is also assumed that the QEMU image is built without a password for
logging in as root - build with EXTRA_USERS_PARAMS = "", if you
install sshpass you can test against images with passwords - set the
environment variable QEMU_USER_SSHPASS to signal this - the password
'root' is assumed.
If you are using  QEMU_USER_SSHPASS you may need to run (as suggested when you get a
   Permission denied (publickey,password).
error)
        ssh-keygen -f ~/.ssh/known_hosts -R [127.0.0.1]:5555
in order to clear known_hosts of any previous build to which you have connected.
sshpass cannot use BatchMode so this initial step is necessary.

If you wish to boot the virtual machine with other options - for
example it redirects the ssh port to 5555 - examine the preface to
geniviTest.py