---
date: 2025-11-23
draft: false
title: "Access Control Principles in Computer Security"
description: "Discover the essential principles of access control in computer security, including RBAC, ABAC, DAC, and MAC. Learn how subjects, objects, access rights, identity federation, and access management work together to protect sensitive data and secure modern systems. Perfect for students, IT professionals, and cybersecurity beginners."
categories:
  - Computer Security
tags:
slug: "access-control-principles-in-computer-security"
authors:
  - "abdo"
comments: true
featuredImage: cover.webp
---

In today’s digital world, securing data is more important than ever. Whether we’re protecting personal information, business systems, or cloud platforms, **access control** plays a central role. Access control determines _who_ can access a resource, _what_ they can do with it, and _under what conditions_.

This guide breaks down the core access control principles, models, and supporting technologies in simple terms—perfect for newcomers, students, and professionals looking for a clear explanation.

---

## **What Is Access Control?**

Access control is the method used to regulate how users, processes, or systems interact with protected resources. These resources can include files, databases, devices, applications, or even physical spaces.

Access decisions depend on three main components:

### **Subjects**

A subject is an entity capable of performing actions.  
Subjects typically fall into three categories:

- **Owner** – The creator or primary controller of a resource
    
- **Group** – A collection of users who share access
    
- **World** – All other users with no special privileges
    

### **Objects**

An object is any resource being protected—such as a file, database table, or service.

### **Access Rights**

Access rights specify _how_ a subject may interact with an object. Common rights include:

- Read
    
- Write
    
- Execute
    
- Delete
    
- Create
    
- Search
    

---

## **Access Control Policies**

Different systems use different policies for determining access. Here are the four major models used today:

---

### **1. Role-Based Access Control (RBAC)**

RBAC assigns permissions based on the **role** a user has within an organization.

For example:

- A _manager_ role might have access to employee performance data.
    
- A _teller_ role in a bank might be allowed to process customer deposits.
    

This model simplifies administration, especially in large organizations, because permissions are tied to positions—not individuals.

---

### **2. Attribute-Based Access Control (ABAC)**

ABAC evaluates access based on **attributes** such as:

- User attributes (e.g., department, clearance level)
    
- Resource attributes (e.g., data classification)
    
- Environmental conditions (e.g., time of day, device type)
    

ABAC is more dynamic and flexible than RBAC. For example, a rule might say:  
_"Allow access only if the user is in the Finance department and requests the data during business hours."_

---

### **3. Discretionary Access Control (DAC)**

DAC gives resource owners the power to decide who can access their resources.

A common implementation of DAC uses an **access matrix**, where:

- One dimension lists subjects
    
- The other lists objects
    
- Each cell shows the allowed rights
    

This is the model used in many traditional operating systems, including UNIX and Windows.

---

### **4. Mandatory Access Control (MAC)**

MAC is the strictest model. Access decisions depend on comparing:

- **Security labels** assigned to objects
    
- **Security clearances** assigned to users
    

Common in government and military environments, MAC ensures that sensitive information can only be accessed by properly cleared individuals.

---

## **Discretionary Access Control in Detail**

DAC often uses an **access matrix** to determine permissions. The matrix maps each subject to each object and displays allowed actions like reading, writing, or executing.

This model supports the idea of **delegation**, where a user with access can grant or remove access for others.

---

## **Access Management**

Access management focuses on how users are verified and granted access to systems or physical locations. It ensures that only authorized individuals can enter secure buildings, use computer systems, or access sensitive information.

An effective enterprise-wide access control system relies on three key elements:

### **1. Resource Management**

Defines rules and requirements for accessing a protected resource.  
This includes the credentials, user attributes, and environmental conditions required.

### **2. Privilege Management**

Handles how user privileges are created and maintained.  
These privileges become part of a user's digital identity and help the system make access decisions.

### **3. Policy Management**

Determines what is allowed or prohibited in an access transaction.  
Policies act as the enforcement backbone of the access control system.

---

## **Identity Federation**

As organizations increasingly collaborate and share data across networks, identity federation has become essential. It allows one organization to **trust digital identities** issued by another.

Identity federation answers two important questions:

1. How do we trust users from external organizations?
    
2. How do we verify our users when they access external resources?
    

Several standards and frameworks support identity federation:

- **OpenID** – Enables authentication across multiple sites using a third-party provider.
    
- **OpenID Foundation (OIDF)** – Maintains and develops OpenID standards.
    
- **Information Card Foundation (ICF)** – Supports digital identity ecosystems.
    
- **Open Identity Trust Framework (OITF)** – A standard for identity and attribute exchange governance.
    
- **Open Identity Exchange (OIX)** – Provides certification for trust frameworks.
    
- **Attribute Exchange Network (AXN)** – Allows secure, large-scale sharing of user attributes.
    

These systems create a trusted environment where identities and privileges can move across organizational boundaries securely.

---

## **Access Control in Practice: A Banking Example**

Banks frequently use RBAC to simplify access decisions. For example:

- A _loan officer_ role might access loan applications.
    
- A _branch manager_ role may approve or modify applications.
    

Users are assigned:

- **Roles** based on their job
    
- **Functions** appropriate to each role
    
- **Access rights** needed to perform those functions
    

This layered approach ensures security, efficiency, and clear separation of duties.

---

## **Summary**

Access control is a foundational part of cybersecurity, ensuring that only the right people—and systems—access the right resources.

Key concepts include:

- **ABAC** – Attribute-driven, dynamic
    
- **RBAC** – Role-driven, easy to manage
    
- **DAC** – Owner-driven, flexible
    
- **MAC** – Clearance-driven, highly secure
    
- **Subjects, objects, and access rights** – Core building blocks
    
- **Identity & access management** – Ensures secure verification and entitlement
    
- **Federated identity systems** – Enable trusted identity sharing across organizations
    

Together, these tools and principles create a secure environment where information stays protected and access remains controlled.
