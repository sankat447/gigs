Creating a custom RPM package for Red Hat Satellite Server involves building an RPM using the `rpmbuild` tool and then adding it to Satellite. Below is a **step-by-step guide**:

---

## **Step 1: Install Required Tools**
Before building an RPM, ensure that you have the necessary tools installed:

```bash
sudo yum install rpm-build rpmdevtools createrepo -y
```

Set up the RPM build environment:

```bash
rpmdev-setuptree
```

This will create the required directory structure in `~/rpmbuild/`:
```
~/rpmbuild/
├── BUILD
├── RPMS
├── SOURCES
├── SPECS
├── SRPMS
└── BUILDROOT
```

---

## **Step 2: Create a Sample Source File**
For demonstration, create a simple script that the RPM will package. Let’s create a `hello.sh` script:

```bash
mkdir ~/rpmbuild/SOURCES
echo -e "#!/bin/bash\necho 'Hello, Red Hat Satellite!'" > ~/rpmbuild/SOURCES/hello.sh
chmod +x ~/rpmbuild/SOURCES/hello.sh
```

---

## **Step 3: Create the RPM SPEC File**
The **SPEC** file defines the metadata, build instructions, and installation steps for the RPM package.

Create a new SPEC file in the `SPECS` directory:

```bash
vim ~/rpmbuild/SPECS/hello.spec
```

Add the following content:

```spec
Name:           hello
Version:        1.0
Release:        1%{?dist}
Summary:        A simple hello script
License:        GPL
Source0:        hello.sh
BuildArch:      noarch

%description
A simple example package containing a script that prints a message.

%prep

%build

%install
mkdir -p %{buildroot}/usr/local/bin
install -m 0755 %{SOURCE0} %{buildroot}/usr/local/bin/hello.sh

%files
/usr/local/bin/hello.sh

%changelog
* Mon Feb 11 2025 Your Name <your.email@example.com> - 1.0-1
- Initial package
```

---

## **Step 4: Build the RPM**
Run the following command to build the package:

```bash
rpmbuild -ba ~/rpmbuild/SPECS/hello.spec
```

If successful, the **RPM package** will be created in the `RPMS` directory:

```bash
ls ~/rpmbuild/RPMS/noarch/
hello-1.0-1.el7.noarch.rpm
```

---

## **Step 5: Create a Custom Repository for Red Hat Satellite**
Move the RPM to a directory that will be used as a **YUM repository**:

```bash
mkdir -p /var/www/html/custom_repo/
cp ~/rpmbuild/RPMS/noarch/hello-1.0-1.el7.noarch.rpm /var/www/html/custom_repo/
```

Create the repository metadata:

```bash
createrepo /var/www/html/custom_repo/
```

Ensure the repository is accessible by configuring `httpd`:

```bash
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

Verify by accessing:
```
http://your-satellite-server/custom_repo/
```

---

## **Step 6: Add Custom RPM to Satellite Server**
1. **Log in to Satellite Web UI**.
2. Go to **Content** → **Products**.
3. Create a new **Product** (e.g., "Custom Software").
4. Create a **Repository** with:
   - **Type**: Yum
   - **URL**: `http://your-satellite-server/custom_repo/`
   - **GPG Key**: (Optional)
5. Sync the repository.

---

## **Step 7: Distribute RPM to Clients**
On a registered client, add the repository:

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/custom.repo
[custom_repo]
name=Custom RPM Repo
baseurl=http://your-satellite-server/custom_repo/
enabled=1
gpgcheck=0
EOF
```

Install the RPM:

```bash
sudo yum clean all
sudo yum install hello
```

Run the script:

```bash
hello.sh
```

---

### **Final Thoughts**
- This guide helps you build, package, and distribute an RPM.
- You can extend the SPEC file for more complex software.
- Consider using **GPG signing** for security.
- Automate RPM creation using **CI/CD pipelines**.

Would you like assistance with automation or troubleshooting? 🚀