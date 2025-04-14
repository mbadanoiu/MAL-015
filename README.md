# MAL-015: Isolation Escape 2 in Ansible Tower

Installations of Ansible Tower on certain Rhel 7/8 setups are potentially vulnerable to “Job Isolation” Escapes that allows an attacker to elevate to the “awx” user from outside the isolated environment.

The following files/setups can be leveraged by attackers in order to perform the Job Isolation Escape vulnerability after [CVE-2021-20253](https://github.com/mbadanoiu/CVE-2021-20253) was patched:
- File “/var/spool/mail/awx”:
  - Can be created by an attacker if there is a SMTP server installed on the target
  - May be created by programs such as “useradd” (in case of a custom awx user)
- In Rhel 7 or lower, in order to install AWX, runtime versions of python (and maybe other packages) need to be installed exposing potentially dangerous writable folders such as “/opt/rh/rh-python36/root/tmp/”
- Folder “/dev/shm/” (only affects systems that do not have the “nosuid” flag on “/dev/shm”)
- Folder “/run/tower/” (only affects systems that do not have the “nosuid” flag on “/run”)
- Other scenarios that have not been found or considered during testing.

### Requirements:

This vulnerability requires:
<br/>
- Being able to execute commands in isolation environment in Ansible Tower
- Having low privileged access to the OS

### Proof Of Concept:

More details and the exploitation process can be found in this [PDF](https://github.com/mbadanoiu/MAL-015/blob/main/Ansible%20-%20MAL-015.pdf).

### Additional Information:

Original Job Isolation Escape Finding: [CVE-2021-20253: Privilege Escalation via Job Isolation Escape in Ansible Tower](https://github.com/mbadanoiu/CVE-2021-20253)

### Timeline:

- Vulnerability was reported to security@ansible.com on 15-Jul-2021
- Discussed vulnerability
- Requested an update on 21-Sep-2021
- Publicly disclosed the vulnerability on 14-Apr-2025
