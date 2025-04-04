# aws-NAT-Gateway-Debug
Awesome! Here's the `.md` version of the NAT Gateway debug template — ready to drop into your Git repo:

---

```markdown
# 🛠️ NAT Gateway Debug Checklist for AWS VPC

Use this checklist to troubleshoot outbound internet connectivity from private subnets using a NAT Gateway.

---

## ✅ Step 1: Identify Route to NAT Gateway in Private Subnet

Check the route table associated with your **private subnet**. You should see:

```
0.0.0.0/0 => nat-xxxxxxxxxxxxxxxxx
```

➡️ This confirms the subnet is **private** and is configured to use a NAT Gateway for outbound traffic.

---

## ✅ Step 2: Check NAT Gateway Subnet

Run the following command to find the subnet where the NAT Gateway is deployed:

```bash
aws ec2 describe-nat-gateways --nat-gateway-ids nat-xxxxxxxxxxxxxxxxx \
  --query "NatGateways[0].SubnetId" --output text
```

---

## ✅ Step 3: Verify That NAT Subnet is Public

The NAT Gateway **must be placed in a public subnet**. That means the subnet’s route table must include:

```
0.0.0.0/0 => igw-xxxxxxxxxxxxxxxxx
```

➡️ This ensures the NAT Gateway has internet access via an **Internet Gateway (IGW)**.

---

## ✅ Step 4: Confirm Elastic IP Assignment

Ensure the NAT Gateway has an **Elastic IP (EIP)** attached. You can check this from the console or via CLI:

```bash
aws ec2 describe-nat-gateways --nat-gateway-ids nat-xxxxxxxxxxxxxxxxx \
  --query "NatGateways[0].NatGatewayAddresses[0].PublicIp"
```

---

## 🧠 Summary

| Component         | Configuration Required                            |
|------------------|----------------------------------------------------|
| Private Subnet    | Route `0.0.0.0/0` to **NAT Gateway**               |
| NAT Subnet        | Route `0.0.0.0/0` to **Internet Gateway (IGW)**   |
| NAT Gateway       | Must have an **Elastic IP**                       |

---

## 🔄 Common Pitfall

If your NAT Gateway is deployed in a **private subnet**, it won't work. It needs to reach the internet to forward traffic on behalf of your private subnet.

---

## 🔗 Bonus: Test Internet from Private Subnet

From a private EC2 instance (with route to NAT), try:

```bash
curl https://www.google.com
```

If it hangs or times out, NAT may not be properly connected to the IGW.

---

```

Let me know if you want to add a section for **App Runner + VPC Connector** scenarios too.
