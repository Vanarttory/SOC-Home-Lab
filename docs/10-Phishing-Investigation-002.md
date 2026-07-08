Priority: High

Time Received: 16:42

Reported by: Finance Analyst

User Statement:

Hi SOC,

I received this email from our CEO asking me to process an urgent vendor payment before the end of business today.

I haven't replied because something felt off.

Could you verify whether this is legitimate?

— James (Finance)


Email Headers:

From: Michael Roberts <m.roberts@contoso.com>

To: James Miller <j.miller@contoso.com>

Subject: URGENT - Vendor Payment Required Today

Date: Tue, 07 Jul 2026 16:38:11 +0300

Message-ID: <98237492@contoso.com>

Authentication-Results:
    spf=pass
    dkim=pass
    dmarc=pass

Return-Path:
m.roberts@contoso.com

Reply-To:
payments.consulting@gmail.com

Received:
from exchange.contoso.com (192.168.15.8)

Email Body:

James,

I need this handled immediately before I board my flight.

Please process a payment of $48,950 USD to our consulting partner.

Bank details will be sent once you confirm.

I'm unavailable by phone while travelling.

Regards,

Michael Roberts
Chief Executive Officer


Additional Evidence:

VirusTotal


Sender domain:

contoso.com

Result:

0/97 detections

WHOIS:


Domain age:

18 years

MXToolbox:


SPF PASS

DKIM PASS

DMARC PASS


No blacklist listings

Exchange Logs:


Login location:

Nairobi

Timestamp:

15:55

Successful login

MFA:

Successful


Microsoft Entra ID:


No impossible travel

No risky sign-ins

No malware alerts

No impossible locations

//The infrastructure can be completely legitimate while the request is malicious.//

Investigative Report 002:

Executive Summary:

On 8 July 2026, the Security Operations Center investigated an email reported by a finance employee requesting an urgent vendor payment.

Although SPF, DKIM, and DMARC authentication all passed and the sender domain appeared legitimate,analysis identified a mismatched Reply-To address redirecting responses to a personal Gmail account. 

The email was determined to be a Business Email Compromise (BEC) attempt designed to manipulate the recipient into authorizing a fraudulent financial transaction.

Incident Overview:

 | Field           | Value                       |
 ------------------|------------------------------
 | Incident Type   |  Business Email Compromise  |
 |   Severity      | High                        |
 |  Status         | Confirmed malicious         |


| Detection Method                   |    User report   |
|------------------------------------|------------------|
| Analyst                            | Victoria Karanja |
| Date                               | 2026-07-08       |


Initial Triage:

Finance employee reported unexpected payment request

Email appeared to originate  from CEO 

User did not interact with email before reporting


Investigation:

Email authentication headers were reviewed. 

SPF, DKIM and DMARC validation all passed, indicating the email originated from an authorized mail server.

Header analysis identified a Reply-To address directing responses to a Gmail account rather than the corporate domain.

Domain reputation checks using VirusTotal and WHOIS did not identify any malicious indicators


Findings:

  Technical Findings

-SPF passed.

-DKIM passed.

-DMARC passed.

-Corporate domain registered for 18 years.

-Reply-To redirected to external Gmail account.

  Behavioral Findings

-Requested urgent financial transaction.

-Claimed sender was unavailable by phone.

-Requested confirmation before sending banking information.

  Indicators of Compromise

| IOC      | Value                                                                 |
| -------- | --------------------------------------------------------------------- |
| Sender   | [m.roberts@contoso.com](mailto:m.roberts@contoso.com)                 |
| Reply-To | [payments.consulting@gmail.com](mailto:payments.consulting@gmail.com) |
| Subject  | URGENT – Vendor Payment Required Today                                |

MITRE ATT&CK:

| Technique             | Why it Applies                                               |
| --------------------- | ------------------------------------------------------------ |
| T1656 – Impersonation | CEO identity abused to gain employee trust                   |
| T1566 – Phishing      | Social engineering through email requesting financial action |

Risk Assesment:

Likelihood : High

Impact : High

Business Risk: Potentail fradulent financial transfer


Recommendations:

Verify financial requests through a second communication channel.

Flag external Reply-To addresses.

Provide user awareness training regarding BEC.

Implement mail flow rules to identify external Reply-To mismatches.

Lessons Learned:

This investigation demonstrates that email authentication alone cannot determine legitimacy. 
Analysts must correlate technical indicators with behavioral characteristics and business context before reaching a conclusion.





  
    

