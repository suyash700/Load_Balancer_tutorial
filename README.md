# 🧭 AWS Application Load Balancer — Header-Based + Path-Based Routing

                  # Overview


🎯 Objective

We will create:

1. 4 EC2 instances

2. 2 Target Groups

3. 1 Application Load Balancer (ALB)
and configure routing rules:

Target Group 1 (TG-1) → Header-based routing

Target Group 2 (TG-2) → Path + Header-based routing (/mobile + specific header)

--------------------
--------------------


## 🚀 Step 1: Create 4 EC2 Instances

Go to EC2 → Instances → Launch Instance.

Choose Amazon Linux 2 / Ubuntu AMI.

Instance Type → t2.micro (Free Tier eligible).

Configure the following for all 4 instances:

Create or select a Security Group → allow port 80 (HTTP) from 0.0.0.0/0.

Add user data to run a small web server.

<img width="1909" height="873" alt="Screenshot 2025-10-31 192448" src="https://github.com/user-attachments/assets/61821134-9b8a-446d-8f96-0bdb72a0ba35" />

Launch 4 instances and name them:

Instance-1 Instance-2 Instance-3 Instance-4

<img width="1919" height="915" alt="Screenshot 2025-10-31 193057" src="https://github.com/user-attachments/assets/5e6ca1a7-7288-4a75-bdfd-0db95fdbabae" />


---------------------------
---------------------------


## 🧩 Step 2: Create Target Group 1 (TG-1)

Go to EC2 → Target Groups → Create Target Group.

Choose Target type = Instance.

<img width="1919" height="918" alt="Screenshot 2025-10-31 193158" src="https://github.com/user-attachments/assets/758a3ac9-1333-4a82-84d2-f403698071b9" />

<img width="1918" height="875" alt="Screenshot 2025-10-31 193215" src="https://github.com/user-attachments/assets/69e9e5ad-5b07-4465-8970-182c7aa35877" />


Protocol = HTTP, Port = 80.

Select your VPC.

Name the TG as target-group-1.

Click Next → Register targets:

Add Instance-1 and Instance-2.

<img width="1919" height="917" alt="Screenshot 2025-10-31 193235" src="https://github.com/user-attachments/assets/b1fb2165-64a5-49aa-86e8-bcb776bebe9d" />

<img width="1915" height="880" alt="Screenshot 2025-10-31 193251" src="https://github.com/user-attachments/assets/468250bb-5390-4ab9-b72b-30f4494d3baa" />

Click Create Target Group.

<img width="1919" height="931" alt="Screenshot 2025-10-31 193314" src="https://github.com/user-attachments/assets/f4f6c84e-b50d-440c-bebd-e85eeced64dc" />

----------------------------------
----------------------------------

## 🌐 Step 3: Create Application Load Balancer (ALB)

Go to EC2 → Load Balancers → Create Load Balancer.

Select Application Load Balancer.

Name → ALB-load-balancer.

Scheme → Internet-facing.

<img width="1919" height="880" alt="Screenshot 2025-10-31 193413" src="https://github.com/user-attachments/assets/c82d09e0-23db-4004-a23d-6d02bed70d7f" />


Listeners → HTTP (Port 80).

<img width="1919" height="882" alt="Screenshot 2025-10-31 193448" src="https://github.com/user-attachments/assets/ae829be0-e1c0-454a-aca2-441ac7dc4e2d" />

Security Group → allow port 80 (HTTP).

Default Target Group → choose TG-1 (temporary default).

Click Create Load Balancer.

<img width="1919" height="927" alt="Screenshot 2025-10-31 193529" src="https://github.com/user-attachments/assets/9f0982bd-770e-4051-a71e-d75115bc65d4" />

-------------------------------
-------------------------------

## 🧱 Step 4: Create Target Group 2 (TG-2)

Go to EC2 → Target Groups → Create Target Group.

Choose Target type = Instance.

Protocol = HTTP, Port = 80.

Name the TG as target-group-2.

Select your VPC.

Click Next → Register targets:

Add Instance-3 and Instance-4.

<img width="1915" height="916" alt="Screenshot 2025-10-31 193832" src="https://github.com/user-attachments/assets/3e4acdd8-534d-4bb8-8ccd-15374cf612a7" />

<img width="803" height="157" alt="Screenshot 2025-10-31 194407" src="https://github.com/user-attachments/assets/0cb2a013-489c-4eb4-b806-adbbaa7ee023" />


<img width="1464" height="354" alt="Screenshot 2025-10-31 194352" src="https://github.com/user-attachments/assets/ac779cde-30a5-46aa-99fc-185dc7be0e25" />

Click Create Target Group.

## ⚙️ Step 5: Configure ALB Listener Rules

Go to:
EC2 → Load Balancers → ALB-load-balancer → Listeners → View/Edit Rules

### Update Security group
<img width="1919" height="898" alt="Screenshot 2025-10-31 194744" src="https://github.com/user-attachments/assets/6d90332a-3e01-4545-a81a-7d6fe645af11" />

<img width="1917" height="915" alt="Screenshot 2025-10-31 194805" src="https://github.com/user-attachments/assets/0d655ce6-e0c7-4c4c-a6a6-dfc61635fe90" />


You’ll see one default rule already forwarding to TG-1.

## 🔹 Rule 1 — Header-Based Routing (TG-1)

Click Add Rule.

If request matches all:

HTTP Header (regex)

<img width="1912" height="897" alt="Screenshot 2025-10-31 200158" src="https://github.com/user-attachments/assets/6284de90-ec03-45ef-a53f-685f07ee51b8" />

Name: User-Agent

Match type: Regex matching

Value: ^MyApp$

Then: Forward to → target-group-1.

<img width="1918" height="895" alt="Screenshot 2025-10-31 195309" src="https://github.com/user-attachments/assets/3851dec7-34ed-454f-bca1-69bf85a0431b" />

<img width="1915" height="916" alt="Screenshot 2025-10-31 195357" src="https://github.com/user-attachments/assets/dfd6c510-484c-48f6-9da5-ae9bd2c1d292" />

Priority → 1.

Save.

-------------------------
-------------------------

### 🔹 Rule 2 — Path + Header-Based Routing (TG-2)

Add another rule.

If request matches all:

Path → /mobile

<img width="1505" height="436" alt="Screenshot 2025-10-31 200940" src="https://github.com/user-attachments/assets/51a84ae2-fd78-4d9a-b64c-0949610b3186" />


HTTP Header (regex)

Name: User-Agent

Match type: Regex matching

Value: ^Chrome$

<img width="1919" height="854" alt="Screenshot 2025-10-31 200632" src="https://github.com/user-attachments/assets/c5fe6b35-11a5-48d0-835a-8d4cc345502a" />


Then: Forward to → target-group-2.

Priority → 2.

Save.

### ✅ Final Rule Order

| Priority | Condition                                       | Action                        |
| -------- | ----------------------------------------------- | ----------------------------- |
| 1        | Header `User-Agent = MyApp`                     | Forward to **TG-1**           |
| 2        | Path `/mobile` AND Header `User-Agent = Chrome` | Forward to **TG-2**           |
| Default  | No match                                        | Forward to **TG-1** (default) |

## Testing

<img width="1482" height="657" alt="Screenshot 2025-10-31 201614" src="https://github.com/user-attachments/assets/30681f7e-1104-4ad0-bfa2-8881a9aff5fa" />

### 🧼 Step 7: Clean Up

After testing:

Delete the ALB

Delete both Target Groups

Terminate all 4 instances

# Author : Suyash Dahitule
