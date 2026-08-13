# Project 1 – Ansible Research Questions

## 📌 Project Overview

This project is a comprehensive research study on **Ansible**, an open-source automation and configuration management platform widely used in modern DevOps environments.

The research covers Ansible architecture, playbooks, modules, idempotence, security, version control, dynamic inventories, scalability, cloud environments, enterprise use cases, error handling, Ansible Galaxy, and dependency management.

The goal of this project is to develop a strong understanding of how Ansible is used to automate infrastructure and configuration management in real-world DevOps environments.

---

## 🎯 Objectives

The main objectives of this research project are to:

- Understand the fundamentals of Ansible.
- Learn how Ansible differs from other configuration management tools.
- Understand the structure of Ansible playbooks and roles.
- Learn how Ansible modules work.
- Understand idempotence in configuration management.
- Learn how Ansible Vault protects sensitive information.
- Understand how Ansible integrates with Git and Infrastructure as Code.
- Explore dynamic inventories and cloud infrastructure discovery.
- Identify best practices for scalable Ansible automation.
- Understand Ansible’s role in multi-cloud and hybrid cloud environments.
- Research real-world enterprise use cases.
- Understand Ansible’s agentless architecture.
- Differentiate between ad-hoc commands and playbooks.
- Learn how Ansible handles errors and retries.
- Understand Ansible Galaxy and reusable automation content.
- Learn how Ansible manages dependencies between tasks and roles.

---

## 📚 Research Questions Covered

This project answers the following 15 research questions:

1. What is Ansible, and how does it differ from other configuration management tools?
2. How can Ansible playbooks be structured, and what role do roles play in organizing automation tasks?
3. What are Ansible modules, and how do they facilitate the execution of tasks on remote systems?
4. How does Ansible handle idempotence, and why is it important in configuration management?
5. What is Ansible Vault, and how does it help in managing sensitive data securely?
6. How can Ansible be integrated with version control systems like Git for Infrastructure as Code management?
7. What are dynamic inventories in Ansible, and how do they enable the automatic discovery of infrastructure resources?
8. What are some best practices for writing efficient and scalable Ansible playbooks and roles?
9. How does Ansible support multi-cloud and hybrid cloud environments, and what are the challenges in managing such infrastructures?
10. What are some real-world case studies or success stories of large-scale Ansible deployments in enterprises?
11. How does Ansible’s agentless architecture work, and what are the advantages and disadvantages of this approach compared to agent-based tools?
12. What is the difference between Ansible ad-hoc commands and playbooks, and when would you use each?
13. How does Ansible handle error handling and retries in automation tasks, and how can you ensure a robust execution flow?
14. What is Ansible Galaxy, and how can it be used to find and share roles and playbooks with the community?
15. How does Ansible manage dependencies between tasks and roles, and what techniques are used to ensure tasks are executed in the correct order?

---

## 🛠️ Technologies and Concepts

The research covers the following technologies and DevOps concepts:

- Ansible
- YAML
- Linux
- SSH
- Git
- GitHub
- Infrastructure as Code (IaC)
- Configuration Management
- Ansible Playbooks
- Ansible Roles
- Ansible Modules
- Ansible Vault
- Dynamic Inventory
- Ansible Galaxy
- CI/CD
- Cloud Computing
- Multi-Cloud
- Hybrid Cloud

---

## 1. What is Ansible, and how does it differ from other configuration management tools?

### What is Ansible?

Ansible is an open-source automation and configuration management platform used to automate tasks such as server configuration, application deployment, software installation, provisioning, and infrastructure management.

Ansible allows DevOps engineers and system administrators to manage multiple servers and infrastructure resources from a central control machine. Instead of manually performing the same configuration tasks on every server, Ansible allows these tasks to be defined as code and executed automatically.

Ansible uses **YAML** files called **playbooks** to describe the desired configuration and the tasks that should be performed. It is commonly used for configuration management, application deployment, infrastructure provisioning, and continuous delivery.

### How Ansible Works

Ansible follows an **agentless architecture**. This means that Ansible does not normally require a separate Ansible agent to be installed on the managed servers.

For Linux and Unix-based systems, Ansible commonly communicates with remote machines using **SSH**. For Windows systems, it can use **WinRM** or other supported connection methods.

The basic architecture consists of:

- **Control Node** – The machine where Ansible is installed and from which automation is executed.
- **Managed Nodes** – The servers or systems that Ansible manages.
- **Inventory** – A list of managed hosts grouped according to the infrastructure.
- **Modules** – Reusable units of code that perform specific tasks.
- **Playbooks** – YAML files that define automation tasks and the desired configuration.
- **Roles** – A structured way of organizing related playbooks, tasks, variables, templates, and files.

A simplified workflow is:

```text
Control Node
     |
     | Ansible
     |
     +-------- SSH --------> Server 1
     |
     +-------- SSH --------> Server 2
     |
     +-------- SSH --------> Server 3
```

### How Ansible Differs from Other Configuration Management Tools

| Feature                  | Ansible                          | Puppet / Chef / SaltStack          |
|--------------------------|----------------------------------|------------------------------------|
| Architecture             | Agentless (SSH / WinRM)          | Usually agent-based                |
| Language                 | YAML (declarative)               | Domain-specific languages (DSL)    |
| Learning curve           | Relatively low                   | Steeper                            |
| Master/agent requirement | No permanent agent required      | Agents typically required          |
| Execution model          | Push-based                       | Often pull-based                   |
| Community content        | Ansible Galaxy                   | Puppet Forge / Chef Supermarket    |

**Key advantages of Ansible:**

- Simple YAML syntax that is easy to read and write.
- No agent installation or maintenance on managed nodes.
- Strong support for multi-cloud, network, and security automation.
- Excellent integration with CI/CD pipelines and Infrastructure as Code practices.

---

## 2. How can Ansible playbooks be structured, and what role do roles play in organizing automation tasks?

### What is an Ansible Playbook?

An Ansible playbook is a YAML file that contains a set of instructions used to automate tasks on managed systems. Playbooks define *what should be done*, *where it should be done*, and sometimes *how it should be done*.

A playbook can contain one or more **plays**, and each play targets a specific group of hosts and contains a collection of tasks.

### Basic Structure of an Ansible Playbook

```yaml
---
- name: Configure Web Servers
  hosts: webservers
  become: true

  vars:
    web_package: nginx

  tasks:
    - name: Install web server
      ansible.builtin.apt:
        name: "{{ web_package }}"
        state: present
        update_cache: true

    - name: Start web server
      ansible.builtin.service:
        name: "{{ web_package }}"
        state: started
        enabled: true
```

### The Role of Roles

As automation grows, putting all tasks in a single playbook becomes hard to maintain. **Roles** provide a standard directory structure that organizes related automation content:

```text
roles/
└── webserver/
    ├── defaults/          # Default variables
    ├── files/             # Static files to copy
    ├── handlers/          # Handlers (e.g., restart service)
    ├── meta/              # Role metadata and dependencies
    ├── tasks/             # Main list of tasks
    ├── templates/         # Jinja2 templates
    └── vars/              # Role variables
```

**Benefits of using roles:**

- Reusability across multiple playbooks and projects.
- Clear separation of concerns.
- Easier testing and versioning.
- Support for role dependencies via `meta/main.yml`.

Playbooks can then simply include roles:

```yaml
- name: Configure web servers
  hosts: webservers
  roles:
    - webserver
```

---

## 3. What are Ansible modules, and how do they facilitate the execution of tasks on remote systems?

### What are Ansible Modules?

Ansible modules are reusable programs or units of code that Ansible uses to perform specific tasks on managed systems. They provide the functionality behind Ansible tasks and allow administrators to perform operations such as installing software, managing files, creating users, configuring services, and interacting with cloud resources.

Instead of requiring administrators to manually execute commands on every server, Ansible modules allow these operations to be defined as tasks in a playbook.

### How Ansible Modules Work

When an Ansible playbook is executed, Ansible connects to the target managed systems and uses the appropriate modules to perform the requested operations.

Example – install Nginx using the `apt` module:

```yaml
- name: Install Nginx
  ansible.builtin.apt:
    name: nginx
    state: present
    update_cache: true
```

**Key characteristics of modules:**

- Most modules are **idempotent** – they check the current state before making changes.
- Modules return structured data (JSON) that Ansible uses for reporting and conditionals.
- There are thousands of modules covering systems, cloud providers, network devices, databases, containers, and more.
- Modules are organized into **collections** (e.g., `ansible.builtin`, `amazon.aws`, `community.general`).

---

## 4. How does Ansible handle idempotence, and why is it important in configuration management?

### What is Idempotence?

Idempotence is the ability to perform the same operation multiple times while producing the same final result as performing it once.

In configuration management, idempotence means that an automation task should only make changes when the current state of a system does not match the desired state.

For example, if a server is supposed to have Nginx installed, running the Ansible playbook repeatedly should not reinstall Nginx every time. Once Nginx is already installed, Ansible should recognize that the desired state has been achieved.

### How Ansible Handles Idempotence

Ansible achieves idempotence primarily through its modules. Many Ansible modules check the current state of a resource before making changes.

```yaml
- name: Ensure Nginx is installed
  ansible.builtin.apt:
    name: nginx
    state: present
```

- If Nginx is already installed → the task reports **ok** (no change).
- If Nginx is missing → the task installs it and reports **changed**.

**Why idempotence matters:**

- Safe to run playbooks repeatedly (e.g., in CI/CD pipelines).
- Prevents unintended side effects and configuration drift.
- Makes automation predictable and easier to debug.
- Supports “desired state” management rather than imperative scripting.

---

## 5. What is Ansible Vault, and how does it help in managing sensitive data securely?

### What is Ansible Vault?

Ansible Vault is a feature of Ansible that allows sensitive information to be encrypted and securely stored within Ansible projects.

In DevOps and infrastructure automation, playbooks often need access to sensitive information such as:

- Passwords
- API keys
- SSH credentials
- Database credentials
- Cloud provider credentials
- Encryption keys
- Tokens

Storing these values as plain text in a Git repository can expose them to unauthorized users. Ansible Vault helps solve this problem by encrypting sensitive files or variables.

### Why is Ansible Vault Important?

Ansible playbooks and configuration files are commonly stored in version control systems such as Git. While version control is useful for tracking infrastructure changes, sensitive information should not normally be committed as readable plain text.

**Example of insecure storage:**

```yaml
database_password: MySecretPassword123
```

**Secure approach with Ansible Vault:**

```bash
# Encrypt a file
ansible-vault encrypt group_vars/all/secrets.yml

# Encrypt a string
ansible-vault encrypt_string 'MySecretPassword123' --name 'database_password'
```

Vault-encrypted content can be decrypted at runtime using a password or a password file:

```bash
ansible-playbook site.yml --ask-vault-pass
# or
ansible-playbook site.yml --vault-password-file ~/.vault_pass
```

**Best practices:**

- Never commit the vault password to the repository.
- Use separate vaults or passwords for different environments.
- Prefer encrypting individual variables when possible.
- Combine with proper access control on the Git repository.

---

## 6. How can Ansible be integrated with version control systems like Git for Infrastructure as Code management?

### Introduction

Ansible can be integrated with version control systems such as Git to manage infrastructure automation as code. This approach is commonly known as **Infrastructure as Code (IaC)**.

Infrastructure as Code allows DevOps engineers to define infrastructure configurations, server configurations, and deployment procedures in files instead of performing the configurations manually.

By storing Ansible playbooks, roles, inventories, and configuration files in Git, teams can track changes, collaborate, review modifications, and restore previous versions when necessary.

### Why Use Git with Ansible?

Git provides version control for Ansible automation. Every change made to an Ansible project can be recorded and tracked.

**Typical project layout in Git:**

```text
ansible-project/
├── README.md
├── ansible.cfg
├── inventory/
│   ├── production/
│   └── development/
├── playbooks/
│   ├── site.yml
│   ├── webserver.yml
│   └── database.yml
├── roles/
│   ├── webserver/
│   └── database/
├── group_vars/
│   └── all/
└── host_vars/
```

**Benefits of Git + Ansible:**

- Full history of infrastructure changes.
- Code review via pull/merge requests.
- Branching strategies for testing changes safely.
- Easy rollback to previous known-good configurations.
- Integration with CI/CD pipelines (GitHub Actions, GitLab CI, Jenkins, etc.).
- Collaboration across teams while maintaining a single source of truth.

---

## 7. What are dynamic inventories in Ansible, and how do they enable the automatic discovery of infrastructure resources?

### What is an Ansible Inventory?

An Ansible inventory is a list of managed hosts that Ansible uses to determine which systems it should connect to and manage.

A basic (static) inventory can be written manually:

```ini
[webservers]
web01
web02

[databases]
db01
db02
```

### What are Dynamic Inventories?

**Dynamic inventories** automatically discover hosts and groups by querying external sources such as cloud providers, CMDB systems, container orchestrators, or custom scripts.

Instead of maintaining a static list of hosts, Ansible can query an API or script at runtime and receive an up-to-date inventory.

**Common dynamic inventory sources:**

- AWS (EC2, ASG)
- Azure
- Google Cloud
- VMware
- OpenStack
- Kubernetes
- Custom scripts / plugins

**Example – AWS EC2 dynamic inventory (plugin):**

```yaml
# inventory/aws_ec2.yml
plugin: amazon.aws.aws_ec2
regions:
  - us-east-1
filters:
  tag:Environment: production
keyed_groups:
  - key: tags.Role
    prefix: role
```

**Benefits:**

- Automatically discovers new instances as they are launched.
- Reflects real-time infrastructure state.
- Reduces manual inventory maintenance.
- Supports tagging and grouping based on cloud metadata.

---

## 8. What are some best practices for writing efficient and scalable Ansible playbooks and roles?

### Introduction

As an Ansible project grows, poorly organized playbooks and roles can become difficult to understand, maintain, test, and troubleshoot. Following best practices helps DevOps teams create automation that is reliable, reusable, secure, and scalable.

### Best Practices

1. **Use a clear project structure**
   ```text
   ansible-project/
   ├── ansible.cfg
   ├── inventory/
   │   ├── production/
   │   └── development/
   ├── playbooks/
   │   └── site.yml
   ├── roles/
   │   ├── webserver/
   │   └── database/
   └── group_vars/
   ```

2. **Prefer roles over large monolithic playbooks.**
3. **Use meaningful names** for plays, tasks, and variables.
4. **Keep variables organized** (`group_vars`, `host_vars`, role defaults).
5. **Use `ansible.builtin` fully-qualified collection names** for clarity and future-proofing.
6. **Make tasks idempotent** – always prefer modules over raw shell commands.
7. **Use handlers** for service restarts instead of restarting on every run.
8. **Leverage tags** for selective execution.
9. **Validate and lint** playbooks with `ansible-lint` and Molecule.
10. **Encrypt secrets** with Ansible Vault.
11. **Document roles** with `README.md` and metadata.
12. **Limit the use of `shell`/`command` modules** – prefer dedicated modules.
13. **Use `become` only when necessary** and scope it tightly.
14. **Test changes** in non-production environments first.
15. **Version roles** and pin collection versions for reproducibility.

---

## 9. How does Ansible support multi-cloud and hybrid cloud environments, and what are the challenges in managing such infrastructures?

### Introduction

Modern organizations often use multiple cloud providers or combine cloud infrastructure with on-premises data centers. This is known as a **multi-cloud** or **hybrid cloud** environment.

Ansible supports these environments by providing automation modules and collections that can interact with different cloud platforms, operating systems, networking devices, containers, and on-premises infrastructure.

This allows DevOps engineers to use a consistent automation approach instead of manually managing each environment separately.

### Multi-Cloud Example

```text
                    Ansible
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
       AWS           Azure          Google Cloud
        |              |              |
      VMs            VMs            VMs
```

### How Ansible Supports Multi-Cloud / Hybrid

- Dedicated collections: `amazon.aws`, `azure.azcollection`, `google.cloud`, `vmware.vmware_rest`, etc.
- Consistent playbook language across providers.
- Dynamic inventories for each cloud.
- Ability to mix on-premises, cloud, network, and container targets in the same playbook.

### Challenges

- **Credential and identity management** across multiple providers.
- **Networking complexity** (VPNs, private links, security groups).
- **Inconsistent APIs and feature sets** between clouds.
- **State drift** if different tools are used in parallel.
- **Cost visibility and governance**.
- **Latency and reliability** of API calls from the control node.
- **Skill requirements** – teams must understand multiple platforms.

**Mitigation strategies:**

- Standardize on Ansible as the single automation layer.
- Use dynamic inventories and tags consistently.
- Centralize secrets with Ansible Vault or external secret managers.
- Apply the same roles and coding standards across all environments.

---

## 10. What are some real-world case studies or success stories of large-scale Ansible deployments in enterprises?

### Introduction

Ansible is widely used by organizations to automate infrastructure management, application deployment, network configuration, and other IT operations. Large enterprises use Ansible to reduce manual work, improve consistency, increase deployment speed, and standardize processes across large environments.

### Notable Examples

**1. NASA**  
NASA has used Ansible to automate aspects of its IT infrastructure and operational environments. Automation helps ensure consistent configuration across many systems and reduces manual administrative effort.

**2. Cisco**  
Cisco uses Ansible extensively for network automation. Network environments contain large numbers of routers, switches, and firewalls. Ansible allows engineers to define configurations as code, deploy standardized settings, collect device information, and perform repetitive changes safely.

**3. Mercedes-Benz**  
Mercedes-Benz has applied Ansible automation to improve IT operations and standardize infrastructure processes across large application and infrastructure environments.

**4. NASA Jet Propulsion Laboratory (JPL)**  
JPL environments require high reliability and repeatability. Ansible’s ability to define configurations as code supports consistent, auditable system configurations.

**5. Major Financial Organizations**  
Banks and financial institutions commonly use Ansible for:

- Server configuration
- Application deployment
- Security baseline enforcement
- Patch management
- User and access management
- Infrastructure provisioning

**6. Telecom and Networking Organizations**  
Telecommunications companies operate large networks. Ansible is used to automate configuration and operational procedures across routers, switches, firewalls, and network services.

```text
                    Ansible
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
     Routers        Switches       Firewalls
        |              |              |
        +--------------+--------------+
                       |
                 Network Services
```

These examples demonstrate that Ansible scales from traditional server configuration to network automation and complex enterprise environments.

---

## 11. How does Ansible’s agentless architecture work, and what are the advantages and disadvantages of this approach compared to agent-based tools?

### Introduction

One of Ansible’s major characteristics is its **agentless architecture**. Unlike many traditional configuration management tools, Ansible generally does not require a dedicated Ansible agent to be installed and continuously running on every managed server.

Instead, Ansible operates from a central control node and connects to managed systems when automation tasks need to be performed.

### How the Agentless Architecture Works

```text
                 Ansible Control Node
                         |
          +--------------+--------------+
          |              |              |
         SSH            SSH            SSH
          |              |              |
          v              v              v
      Server 1        Server 2        Server 3
```

- For Linux/Unix: SSH (or other connection plugins).
- For Windows: WinRM (or SSH).
- Temporary modules are copied to the remote system, executed, and cleaned up.

### Advantages

- No agent installation or ongoing agent maintenance.
- Lower resource usage on managed nodes.
- Easier to get started (especially in existing environments).
- Reduced attack surface (no permanently running agent process).
- Works well with systems that restrict agent installation.

### Disadvantages

- Requires network connectivity (SSH/WinRM) at execution time.
- Performance can be slower for very large fleets compared to some agent-based pull models.
- Relies on the security of the connection method (SSH keys, certificates).
- Some advanced continuous monitoring/remediation use cases may be better served by agents.

**Comparison summary:** Agentless tools (Ansible) favor simplicity and low overhead; agent-based tools (Puppet, Chef, Salt) often favor continuous enforcement and scale in highly dynamic environments.

---

## 12. What is the difference between Ansible ad-hoc commands and playbooks, and when would you use each?

### Introduction

Ansible provides two common ways to perform automation tasks: **ad-hoc commands** and **playbooks**.

Both can be used to manage remote systems, but they are designed for different purposes. Ad-hoc commands are useful for quick, one-time operations, while playbooks are better suited for repeatable, structured, and complex automation.

### Ad-Hoc Commands

Ad-hoc commands are executed directly from the command line and do not require a playbook file.

```bash
# Check connectivity
ansible all -m ansible.builtin.ping

# Install a package
ansible webservers -m ansible.builtin.apt -a "name=nginx state=present" -b

# Restart a service
ansible webservers -m ansible.builtin.service -a "name=nginx state=restarted" -b
```

**Best used for:**

- Quick checks and one-off tasks.
- Gathering information.
- Emergency or temporary operations.
- Learning and testing modules.

### Playbooks

Playbooks are YAML files that define a structured set of plays and tasks. They support variables, handlers, roles, conditionals, loops, and complex orchestration.

**Best used for:**

- Repeatable and version-controlled automation.
- Complex multi-step configurations.
- Production deployments.
- Sharing and collaborating on automation.
- CI/CD pipelines.

**Rule of thumb:**  
Use ad-hoc commands for exploration and one-time actions. Use playbooks for anything that needs to be reliable, reviewed, or repeated.

---

## 13. How does Ansible handle error handling and retries in automation tasks, and how can you ensure a robust execution flow?

### Introduction

Automation tasks can sometimes fail because of network problems, incorrect configurations, unavailable services, temporary resource failures, or unexpected conditions.

Ansible provides several features that allow DevOps engineers to detect failures, control how errors are handled, retry tasks, and create reliable automation workflows.

### Key Error-Handling Mechanisms

| Directive / Construct     | Purpose                                      |
|---------------------------|----------------------------------------------|
| `failed_when`             | Custom failure conditions                    |
| `changed_when`            | Control when a task is reported as changed   |
| `ignore_errors: true`     | Continue even if the task fails              |
| `block` / `rescue` / `always` | Structured exception handling             |
| `until` + `retries`       | Retry a task until a condition is met        |
| `max_fail_percentage`     | Stop a play if too many hosts fail           |
| `any_errors_fatal`        | Abort the entire play on first failure       |

### Examples

**Retry until a service is ready:**

```yaml
- name: Wait for application to become ready
  ansible.builtin.uri:
    url: http://localhost:8080/health
    status_code: 200
  register: result
  until: result.status == 200
  retries: 10
  delay: 5
```

**Block / Rescue / Always:**

```yaml
- block:
    - name: Attempt risky operation
      ansible.builtin.command: /opt/app/deploy.sh
  rescue:
    - name: Handle failure
      ansible.builtin.debug:
        msg: "Deployment failed – rolling back"
  always:
    - name: Cleanup
      ansible.builtin.file:
        path: /tmp/deploy.lock
        state: absent
```

**Best practices for robust execution:**

- Prefer modules that are naturally idempotent.
- Use `register` + `failed_when` for precise control.
- Add meaningful error messages and logging.
- Design plays so that partial failures leave systems in a known state.
- Test failure scenarios intentionally.

---

## 14. What is Ansible Galaxy, and how can it be used to find and share roles and playbooks with the community?

### Introduction

**Ansible Galaxy** is a community platform and collection of tools used to find, download, reuse, and share Ansible automation content.

It provides access to reusable content such as **roles** and **collections**, allowing DevOps engineers to avoid creating common automation components from scratch.

### What is an Ansible Role?

An Ansible role is a structured collection of automation components:

```text
nginx/
├── defaults/
├── handlers/
├── tasks/
├── templates/
├── files/
└── meta/
```

### Using Ansible Galaxy

```bash
# Search for roles
ansible-galaxy search nginx

# Install a role
ansible-galaxy install geerlingguy.nginx

# Install from a requirements file
ansible-galaxy install -r requirements.yml

# Install a collection
ansible-galaxy collection install community.general
```

**Example `requirements.yml`:**

```yaml
---
roles:
  - name: geerlingguy.nginx
    version: 3.1.0

collections:
  - name: community.general
    version: 8.0.0
```

**Benefits:**

- Accelerate development by reusing proven content.
- Share internal roles and collections across teams.
- Discover community best practices.
- Version and pin dependencies for reproducible environments.

Galaxy also supports publishing your own roles and collections so others (or other teams) can consume them.

---

## 15. How does Ansible manage dependencies between tasks and roles, and what techniques are used to ensure tasks are executed in the correct order?

### Introduction

In Ansible automation, tasks often depend on other tasks or resources being completed first. For example, an application cannot normally be started before its required package has been installed and its configuration has been created.

Ansible provides several mechanisms for controlling task execution and managing dependencies.

### Task Execution Order

By default, Ansible executes tasks **in the order in which they appear** in a playbook.

```yaml
---
- name: Configure Application Server
  hosts: appservers
  become: true

  tasks:
    - name: Install application package
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Copy application configuration
      ansible.builtin.copy:
        src: app.conf
        dest: /etc/nginx/conf.d/app.conf

    - name: Start application service
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

### Techniques for Managing Dependencies

1. **Implicit ordering** – simply list tasks in the correct sequence.
2. **Handlers** – notify handlers that run only when a task reports `changed`.
3. **Role dependencies** – declare dependencies in `meta/main.yml`:
   ```yaml
   dependencies:
     - role: common
     - role: firewall
   ```
4. **`include_role` / `import_role`** – dynamically or statically include roles.
5. **`when` conditionals** – run tasks only when prerequisites are met.
6. **`block` / `rescue`** – group related tasks and handle failures together.
7. **Tags** – selectively execute groups of tasks.
8. **`serial` and `max_fail_percentage`** – control rolling updates and failure tolerance.

These mechanisms together allow precise control over execution order and inter-task / inter-role dependencies.

---

## 📝 Conclusion

Ansible is a powerful automation and configuration management tool that helps DevOps engineers manage infrastructure efficiently and consistently. Its agentless architecture, reusable modules, playbooks, roles, idempotence, security features, and integration with Git make it suitable for both small environments and large enterprise infrastructures.

Through this research, a solid understanding was gained of how Ansible can be used for configuration management, application deployment, Infrastructure as Code, cloud automation, network automation, and multi-cloud environments. Important concepts such as Ansible Vault, dynamic inventories, error handling, Ansible Galaxy, role dependencies, and best practices for building scalable automation were also explored.

Overall, Ansible plays an important role in modern DevOps by reducing manual work, improving consistency, increasing deployment efficiency, and enabling infrastructure to be managed as code.

---

## 🔗 References

1. [Ansible Documentation](https://docs.ansible.com/) – Official Ansible documentation covering installation, configuration, playbooks, modules, roles, and automation.
2. [Ansible User Guide](https://docs.ansible.com/ansible/latest/user_guide/index.html) – Detailed guide covering core Ansible concepts and usage.
3. [Ansible Modules Documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/) – Official documentation for Ansible’s built-in modules.
4. [Ansible Vault Documentation](https://docs.ansible.com/ansible/latest/vault_guide/index.html) – Documentation on encrypting and managing sensitive data with Ansible Vault.
5. [Ansible Galaxy](https://galaxy.ansible.com/) – Platform for discovering and sharing Ansible roles and collections.
6. [Red Hat Ansible Automation Platform](https://www.redhat.com/en/technologies/management/ansible) – Information about enterprise Ansible automation and its use in IT environments.
7. [Git Documentation](https://git-scm.com/doc) – Documentation for Git and version control concepts used with Infrastructure as Code.
8. [GitHub Documentation](https://docs.github.com/) – Documentation covering repositories, collaboration, version control, and software development workflows.
9. [Ansible Community Documentation](https://docs.ansible.com/projects/ansible-community/) – Community resources and documentation for the Ansible ecosystem.


