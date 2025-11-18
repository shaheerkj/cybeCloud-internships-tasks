

# Ticket 1: [Git] Audit main and develop branch protection rules for mandatory checks.

#### Branch Protection Rules:

- Purpose: Settings in Git repo hosting services that are used to prevent unauthorized changes, accidental deletions, or risky operations on important branches (usually `main` or `develop`). GitHub protected branches are an excellent solution for project leaders looking to keep tabs on top-priority code changes and updates without discouraging distributed development teams from freely contributing code. Team leaders simply set specific rules for protected branches.

#### Mandatory Checks

- Definition: Mandatory checks refer to automated requirements that must pass before code can be merged into a protected branch.

- **Common Checks:**
  
  - Status checks (CI/CD pipelines, automated tests) must succeed. [Why does branch protection matter?](https://blog.mergify.com/github-branch-protection-what-it-is-and-why-it-matters/)​
  
  - Pull request (PR) reviews and approvals are required.
  
  - Conversation must be resolved (no unresolved comments).
  
  - Commits may need to be signed for verification.​
  
  - The branch must not be behind on its base (must be up to date).

# Ticket 2: [Scripting] Write a Python script to check SSL certificate expiration dates weekly.

```python
import socket
import ssl
from datetime import datetime,date

hostname = 'github.com'
port = 443  

context = ssl.create_default_context()

with socket.create_connection((hostname, port)) as sock:
    with context.wrap_socket(sock, server_hostname=hostname) as ssock:
        cert = ssock.getpeercert()

        print(f"Connected to {hostname}")
        print("\n=== SSL Certificate Details ===")

        date_str=cert["notAfter"].split(" GMT")[0]
        fmt = "%b %d %H:%M:%S %Y"
        expiry_date = datetime.strptime(date_str, fmt).date()
        date=date.today()
        print(date)
        diff = (expiry_date-date)
        print("The certificate expires in",diff.days,"days.")
        print(diff)
```

## Task: Read The Phoenix Project and the three flows.

The three theories:

- *The Theory of Constraints (TOC)* is a management philosophy that focuses on identifying and eliminating the biggest obstacle, or constraint, in a process or system to improve overall performance. Developed by [Eliyahu M. Goldratt](https://www.google.com/search?sca_esv=63701f3dff9dda34&sxsrf=AE3TifOwUA1y2P-K-KLTD-XjM77BvD5m3A%3A1762275903193&q=Eliyahu+M.+Goldratt&sa=X&ved=2ahUKEwiX2tKp_diQAxUTdqQEHd4IBpwQxccNegQIMRAB&mstk=AUtExfCtnVQCsTHIv25nyRm0S93MQY2xHVR_qxKMPDwGG4bwcDF0MBRWsj_DLf7uTW2g2yGPBGtSi5mvwbGul3BTwzdfvHa-yYYdyTIPJnaU07Fq5WR6nsJKPaM8VdD1tsVYd32wtrRKFyR5HuPE0-AjdSvjzM-KsLmITHeTj6Qdr37d_TZSES-uNt8WMM2tSCYp6ncb2t_wrZ8v5UctiUUIBB_K-k7yC6siC4CfzcLFs9E5LuWcn227GerY9vglArxD_8rx9EfCpXAvumQivKgfOWk0UchX-Knw0nvmeRYBsvBdqQ&csui=3), it operates on the principle that every system has at least one constraint, and its performance is limited by this "weakest link". By systematically improving the constraint, an organization can increase its throughput and achieve its goals.

- Lean production, or the Toyota Production System (TPS), is a management approach that maximizes customer value by eliminating waste and continuously improving processes. It aims to produce high-quality products with minimal resources, low costs, and short lead times by focusing on efficient flow and customer demand. Key concepts include *Just-in-Time* production and *jidoka* (automation with a human touch), guided by the pillars of continuous improvement (*kaizen*) and respect for people.

- Total Quality Management (TQM) is a management approach where all employees, from top management to frontline workers, work together to continuously improve products and services to ensure long-term success through customer satisfaction. It is a systematic approach that uses data and a focus on customer needs to improve quality across all organizational processes, aiming to reduce errors, decrease waste, and increase efficiency.

The Three ways:

- The First Way helps us understand how to create fast flow of work as it moves from
  Development into IT Operations, because that’s what’s between the business and the customer.

- The Second Way shows us how to shorten and
  amplify feedback loops, so we can fix quality at the source and avoid
  rework.

- And the Third Way shows us how to create a culture that simultaneously fosters experimentation, learning from failure, and understanding that repetition and practice are the prerequisites to mastery

### The Three Ways (Flows) - In detail

The book distills DevOps principles into **The Three Ways**, which are systemic flows that must be optimized to achieve high-performing IT organizations. Think of them as circulatory systems for value, feedback, and learning.

1. **The First Way: Systems Thinking / Flow (Left to Right)** Focus on the **entire value stream** from concept (customer need or business idea) to cash (delivered value). Maximize flow by:
   - Reducing batch sizes (small, frequent releases instead of big-bang projects).
   - Eliminating bottlenecks and handoffs.
   - Limiting WIP (work in progress) to prevent overload.
   - Automating repetitive tasks (build/test/deploy pipelines). *Goal*: Accelerate lead time from idea to production while maintaining quality. Example: Deploying code 100 times/day instead of once/quarter.
2. **The Second Way: Amplify Feedback Loops (Right to Left)** Create short, fast feedback from production back to development and operations to catch problems early and prevent rework.
   - Telemetry/monitoring everywhere (application, infrastructure, customer behavior).
   - Automated testing at every stage (unit, integration, performance, security).
   - "Andon cords" (stop-the-line culture when defects are found).
   - Peer reviews, blameless post-mortems, and A/B testing. *Goal*: Prevent defects from moving downstream and institutionalize rapid learning. Example: If a deploy breaks production, roll back in seconds and fix the root cause immediately.
3. **The Third Way: Culture of Continual Experimentation and Learning** Foster a **high-trust, generative culture** that allocates time for mastery, risk-taking, and improvement.
   - 20% time for kaizen (continuous improvement) and technical debt reduction.
   - Chaos engineering (intentionally breaking things in production to build resilience).
   - Blameless post-mortems and sharing failures as learning opportunities.
   - Cross-functional teams (Dev + Ops + Security) with shared goals. *Goal*: Build organizational resilience and innovation by treating failure as a normal part of experimentation. Example: Running "game days" to simulate outages and improve mean-time-to-recover (MTTR).

# Ticket 3: [Git] Implement and enforce commit signing using GPG keys across the organization.

[Link to Article](https://medium.com/@shaheerkj/ensuring-trust-in-your-code-signing-git-commits-with-gpg-832120822216)

# Ticket 4: [Git] Configure git-bisect to quickly diagnose an ancient bug introduced months ago.

[Link to Article](https://medium.com/@shaheerkj/git-bisect-effectively-pinpoint-bugs-introduced-in-earlier-commits-36cdf9a5a439)

# Ticket 6: [Scripting] 

```python
import requests
api_endpoints = [
    "https://shop.example.com/api/users",
    "https://shop.example.com/api/products",
    "https://shop.example.com/api/orders"
]
def validate_endpoints(endpoints):
    for url in endpoints:
        try:
            response = requests.get(url)
            if response.ok:
                print(f"{url} --> {response.status_code}")
            else:
                print(f"{url} --> Returned {response.status_code}")
        except requests.exceptions.RequestException as e:
            print(f"{url} --> Failed ({e})")

if __name__ == "__main__":
    validate_endpoints(api_endpoints)

```
