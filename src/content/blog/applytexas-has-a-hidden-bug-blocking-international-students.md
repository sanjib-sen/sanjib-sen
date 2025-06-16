---
title: A complete list of National Level Olympiads in Bangladesh
author: Sanjib Kumar Sen
featured: true
draft: false
pubDatetime: 2025-06-16T02:05:51Z
tags:
  - students
  - texas
  - phd
  - admission
  - software-engineering
  - international-students
description: "ApplyTexas Has a Hidden Bug Blocking International Students - How I Reverse-Engineered the Website Network Calls to Solve It for My Own PhD Application"
---

# ApplyTexas Has a Hidden Bug Blocking International Students - How I Reverse-Engineered the Website Network Calls to Solve It for My Own PhD Application

## 🚨 **Deadline Approaching. My PhD Dreams Were in Jeopardy.**

Picture this: You're an international student with **very little time left** to submit your PhD application to Texas universities. Your F1 visa timeline is already tight. You need that I-20 document ASAP or your entire academic future crumbles.

Then ApplyTexas decides to throw a wrench in everything.

## **The Digital Nightmare Begins**

After spending days crafting the perfect application, I hit submit and... 

**ERROR. "About You" section incomplete.**

*Wait, what?* I'd filled every single field. Triple-checked everything. The main dashboard shows 100% completion of "Core Questions". The form looked complete, but the system was saying otherwise.

<img width="800" alt="Screenshot 2025-06-17 at 12 24 52 AM" src="https://github.com/user-attachments/assets/fe219a62-5723-4868-94ae-8071e9dd14b1" />

*Caption: My contact form, completely filled out but somehow "incomplete"*

With my SWE background, I knew something was wrong. But I also knew I had **very limited time** to either fix this or watch my PhD opportunity slip away.

## **Plot Twist: The Hidden System Flaw**

After hours of debugging (yes, I was THAT desperate, also thanks to my adrenaline rush whenever I find a bug), I discovered something that shocked me:

✅ **Put in ANY US address** → Application submits perfectly  
❌ **Use my real Bangladesh address** → System breaks  

The system had a critical flaw that specifically affected international students - and nobody seemed to know about it.

## **Going Full Detective Mode**

Time for some digital forensics. I fired up Edge DevTools like I always do everytime I see a bug (force of habit, thanks work).

<img width="1003" alt="Screenshot 2025-06-17 at 12 32 54 AM" src="https://github.com/user-attachments/assets/c69ca131-3ce0-469d-8be7-7b75b77d12de" />

*Caption: The smoking gun - US address sends state "AL", Bangladesh sends "null"*

**THE DISCOVERY:** 
- US students: `"permanentState": "AL"` ✅
- International students: `"permanentState": null` ❌

The backend was rejecting null state values, but the frontend only sent states for US addresses. **Classic validation mismatch that nobody had tested for international users.**

## **The Make-or-Break Decision: Hack or Give Up?**

With my deadline rapidly approaching, I had two choices:
1. Accept defeat and miss the opportunity
2. Manually modify the API request and fix it myself

Guess which one I chose? 💻

## **The 30-Second Engineering Solution That Saved Everything**

Here's where my SWE skills came in clutch:

1. **Intercepted** the failed network request
2. **Modified** the payload: `"permanentState": null` → `"permanentState": "Dhaka"`
3. **Resent** the request manually
4. **BOOM** - Application status changed to complete

<img width="555" alt="Screenshot 2025-06-17 at 12 38 59 AM" src="https://github.com/user-attachments/assets/7bd17c17-8936-452f-a382-22153ab818a1" />

<img width="1191" alt="Screenshot 2025-06-17 at 12 38 02 AM" src="https://github.com/user-attachments/assets/cb662e9b-29cc-45e1-a023-22342e5e9564" />

*Caption: The magic moment - Modifying the Form Submission Request Payload and getting success"*

## **Victory Screenshot**

<img width="866" alt="Screenshot 2025-06-17 at 12 33 39 AM" src="https://github.com/user-attachments/assets/a01723ee-00c4-4400-af9b-b1b79e5627c0" />

*Caption: All sections finally showing as complete - PhD dreams back on track!*

## **The Real Problem**

This bug has probably affected **THOUSANDS** of international students applying to Texas universities. Students who don't have coding backgrounds just... get stuck.

How many brilliant minds never made it to Texas because of this overlooked technical issue?

```javascript
// The invisible barrier
"permanentState": null  // 💀 Applications die here
```

## **For My Fellow International Students**

If you're reading this in a panic about your application deadline, here's your technical lifeline:

### **The Developer Workaround:**
1. Open DevTools (F12)
2. Submit your international address 
3. Find the failed POST request
4. Right-click → "Edit and Resend"
5. Change `"permanentState": null` to `"permanentState": "YourCity"`
6. Hit Send
7. Watch your application magically become "complete"

## **The Happy Ending**

**Update:** I successfully got into my PhD program and am now working on my F1 visa process.

But I keep thinking - what if I wasn't a software engineer? What if I'd just accepted the broken system and given up?

## **Dear ApplyTexas Development Team**

If you're reading this: there's a simple fix that could help thousands of students:

```diff
// Current problematic logic
- Backend expects permanentState for ALL countries
+ Backend should allow null permanentState for non-US countries
```

Or alternatively:
```diff
// Frontend fix
- Only send permanentState for US addresses
+ Send permanentState for all countries (use city/region for international)
```

**💡 Pro Tip:** Always check the Network tab when forms mysteriously "fail." That error message might be hiding a simple technical issue with an even simpler solution.

---
