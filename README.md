# RFC6749

IETF(국제 표준화 기구)의 RFC 6740(The OAuth 2.0 Authorization Framework)을 공부하기 위함

# The OAuth 2.0 Authorization Framework

## Abstract

OAuth 2.0 인증 프레임 워크를 사용하면 리소스 소유자와 HTTP 서비스 간의 승인 상호 작용을 조정하거나 타사 애플리케이션이 자신을 대신하여 액세스 권한을 얻습니다. 이 사양은 [RFC 5849](https://tools.ietf.org/html/rfc5849)에 설명 된 OAuth 1.0 프로토콜을 대체하고 폐기합니다.

## Status of This Memo

이것은 인터넷 표준 추적 문서입니다. 이 문서는 IETF (Internet Engineering Task Force)의 제품입니다. IETF 커뮤니티의 합의를 나타냅니다. 공개 검토를 받았으며 IESG (Internet Engineering Steering Group)의 게시 승인을 받았습니다. 인터넷 표준에 대한 추가 정보는 RFC 5741의 섹션 2에서 확인할 수 있습니다.이 문서의 현재 상태, 정오표 및 이에 대한 피드백 제공 방법에 대한 정보는 http://www.rfc-editor.org/info/rfc6749 에서 얻을 수 있습니다.

## Copyright Notice

Copyright (c) 2012 IETF Trust and the persons identified as the document authors. All rights reserved.
This document is subject to [BCP 78](https://tools.ietf.org/html/bcp78) and the IETF Trust's Legal Provisions Relating to IETF Documents(http://trustee.ietf.org/license-info) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document. Code Components extracted from this document must include Simplified BSD License text as described in Section 4.e of the Trust Legal Provisions and are provided without warranty as described in the Simplified BSD License.

# Table of Contents

- [1. Introduction](#1-Introduction)
  - [1.1. Roles](#11-Roles)
  - [1.2. Protocol Flow](#12-Protocol-Flow)
  - [1.3. Authorization Grant](#13-Authorization-Grant)
    - [1.3.1. Authorization Code](#131-Authorization-Code)
    - [1.3.2. Implicit](#132-Implicit)
    - [1.3.3. Resource Owner Password Credentials](#133-Resource-Owner-Password-Credentials)
    - [1.3.4. Client Credentials](#134-Client-Credentials)
  - [1.4. Access Token](#14-Access-Token)
  - [1.5. Refresh Token](#15-Refresh-Token)
  - [1.6. TLS Version](#16-TLS-Version)
  - [1.7. HTTP Redirections](#17-HTTP-Redirections)
  - [1.8. Interoperability](#18-Interoperability)
  - [1.9. Notational Conventions](#19-Notational-Conventions)
- [2. Client Registration](#2-Client-Registration) - [2.1. Client Types](#21-Client-Types) - [2.2. Client Identifier](#22-Client-Identifier) - [2.3. Client Authentication](#23-Client-Authentication) - [2.3.1. Client Password](#231-Client-Password) - [2.3.2. Other Authentication Methods](#232-Other-Authentication-Methods) - [2.4. Unregistered Clients](#24-Unregistered-Clients)
  [3. Protocol Endpoints](#3-Protocol-Endpoints) - [3.1. Authorization Endpoint](#31-Authorization-Endpoint) - [3.1.1.Response Type](#311-Response-Type) - [3.1.2. Redirection Endpoint](#312-Redirection-Endpoint) - [3.2. Token Endpoint](#32-Token-Endpoint) - [3.2.1. Client Authentication](#321-Client-Authentication) - [3.3. Access Token Scope](#33-Access-Token-Scope)
- [4. Obtaining Authorization](#4-Obtaining-Authorization)
  - [4.1. Authorization Code Grant](#41-Authorization-Code-Grant)
    - [4.1.1. Authorization Request](#411-Authorization-Request)
    - [4.1.2. Authorization Response](#412-Authorization-Response)
    - [4.1.3. Access Token Request](#413-Access-Token-Request)
    - [4.1.4. Access Token Response](#414-Access-Token-Response)
  - [4.2. Implicit Grant](#42-Implicit-Grant)
    - [4.2.1. Authorization Request](#421-Authorization-Request)
    - [4.2.2. Access Token Response](#422-Access-Token-Response)
  - [4.3. Resource Owner Password Credentials Grant](#43-Resource-Owner-Password-Credentials-Grant)
    - [4.3.1. Authorization Request and Response](#431-Authorization-Request-and-Response)
    - [4.3.2. Access Token Request](#432-Access-Token-Request)
    - [4.3.3. Access Token Response](#433-Access-Token-Response)
  - [4.4. Client Credentials Grant](#44-Client-Credentials-Grant)
    - [4.4.1. Authorization Request and Response](#441-Authorization-Request-and-Response)
    - [4.4.2. Access Token Request](#442-Access-Token-Request)
    - [4.4.3. Access Token Response](#443-Access-Token-Response)
  - [4.5. Extension Grants](#45-Extension-Grants)
- [5. Issuing an Access Token](#5-Issuing-an-Access-Token)
  - [5.1. Successful Response](#51-Successful-Response)
  - [5.2. Error Response](#52-Error-Response)
- [6. Refreshing an Access Token](#6-Refreshing-an-Access-Token)
- [7. Accessing Protected Resources](#7-Accessing-Protected-Resources)
  - [7.1. Access Token Types](#71-Access-Token-Types)
  - [7.2. Error Response](#72-Error-Response)
- [8. Extensibility](#8-Extensibility)
  - [8.1. Defining Access Token Types](#81-Defining-Access-Token-Types)
  - [8.2. Defining New Endpoint Parameters](#82-Defining-New-Endpoint-Parameters)
  - [8.3. Defining New Authorization Grant Types](#83-Defining-New-Authorization-Grant-Types)
  - [8.4. Defining New Authorization Endpoint Response Types](#84-Defining-New-Authorization-Endpoint-Response-Types)
  - [8.5. Defining Additional Error Codes](#85-Defining-Additional-Error-Codes)
- [9. Native Applications](#9-Native-Applications)
- [10. Security Considerations](#10-Security-Considerations)
  - [10.1. Client Authentication](#101-Client-Authentication)
  - [10.2. Client Impersonation](#102-Client-Impersonation)
  - [10.3. Access Tokens](#103-Access-Tokens)
  - [10.4. Refresh Tokens](#104-Refresh-Tokens)
  - [10.5. Authorization Codes](#105-Authorization-Codes)
  - [10.6. Authorization Code Redirection URI Manipulation](#106-Authorization-Code-Redirection-URI-Manipulation)
  - [10.7. Resource Owner Password Credentials](#107-Resource-Owner-Password-Credentials)
  - [10.8. Request Confidentiality](#108-Request-Confidentiality)
  - [10.9. Ensuring Endpoint Authenticity](#109-Ensuring-Endpoint-Authenticity)
  - [10.10. Credentials-Guessing Attacks](#1010-Credentials-Guessing-Attacks)
  - [10.11. Phishing Attacks](#1011-Phishing-Attacks)
  - [10.12. Cross-Site Request Forgery](#1012-Cross-Site-Request-Forgery)
  - [10.13. Clickjacking](#1013-Clickjacking)
  - [10.14. Code Injection and Input Validation](#1014-Code-Injection-and-Input-Validation)
  - [10.15. Open Redirectors](#1015-Open-Redirectors)
  - [10.16. Misuse of Access Token to Impersonate Resource Owner in Implicit Flow](#1016-Misuse-of-Access-Token-to-Impersonate-Resource-Owner-in-Implicit-Flow)
- [11. IANA Considerations](#11-IANA-Considerations)
  - [11.1. OAuth Access Token Types Registry](#111-OAuth-Access-Token-Types-Registry)
    - [11.1.1. Registration Template](#1111-Registration-Template)
  - [11.2. OAuth Parameters Registry](#112-OAuth-Parameters-Registry)
    - [11.2.1. Registration Template](#1121-Registration-Template)
    - [11.2.2. Initial Registry Contents ](#1122-Initial-Registry-Contents)
  - [11.3. OAuth Authorization Endpoint Response Types Registry](#113-OAuth-Authorization-Endpoint-Response-Types-Registry)
    - [11.3.1. Registration Template](#1131-Registration-Template)
    - [11.3.2. Initial Registry Contents](#1132-Initial-Registry-Contents)
  - [11.4. OAuth Extensions Error Registry](#114-OAuth-Extensions-Error-Registry)
    - [11.4.1. Registration Template](#1141-Registration-Template)
- [12. References](#12-References)
  - [12.1. Normative References](#121-Normative-References)
  - [12.2. Informative References](#122-Informative-References)
- [Appendix A. Augmented Backus-Naur Form (ABNF) Syntax](#Appendix-A-Augmented-Backus-Naur-Form-ABNF-Syntax)
  - [A.1. "client_id" Syntax](#A1-client_id-Syntax)
  - [A.2. "client_secret" Syntax](#A2-client_secret-Syntax)
  - [A.3. "response_type" Syntax](#A3-response_type-Syntax)
  - [A.4. "scope" Syntax](#A4-scope-Syntax)
  - [A.5. "state" Syntax](#A5-state-Syntax)
  - [A.6. "redirect_uri" Syntax](#A6-redirect_uri-Syntax)
  - [A.7. "error" Syntax](#A7-error-Syntax)
  - [A.8. "error_description" Syntax](#A8-error_description-Syntax)
  - [A.9. "error_uri" Syntax](#A9-error_uri-Syntax)
  - [A.10. "grant_type" Syntax](#A10-grant_type-Syntax)
  - [A.11. "code" Syntax](#A11-code-Syntax)
  - [A.12. "access_token" Syntax](#A12-access_token-Syntax)
  - [A.13. "token_type" Syntax](#A13-token_type-Syntax)
  - [A.14. "expires_in" Syntax](#A14-expires_in-Syntax)
  - [A.15. "username" Syntax](#A15-username-Syntax)
  - [A.16. "password" Syntax](#A16-password-Syntax)
  - [A.17. "refresh_token" Syntax](#A17-refresh_token-Syntax)
  - [A.18. Endpoint Parameter Syntax](#A18-Endpoint-Parameter-Syntax)
- [Appendix B. Use of application/x-www-form-urlencoded Media Type](#Appendix-B-Use-of-applicationx-www-form-urlencoded-Media-Type)

## 1. Introduction

전통적인 클라이언트-서버 인증 모델에서 클라이언트는 리소스 소유자의 자격 증명을 사용하여 서버에서 인증함으로써 서버에서 액세스가 제한된 리소스 (보호 된 리소스)를 요청합니다. 제한된 리소스에 대한 타사 애플리케이션 액세스를 제공하기 위해 리소스 소유자는 해당 자격 증명을 타사와 공유합니다. 이로 인해 몇 가지 문제와 제한 사항이 발생합니다:

o 타사 응용 프로그램은 리소스 소유자의 자격 증명을 나중에 사용할 수 있도록 일반적으로 일반 텍스트의 암호를 저장하는 것이 필요하다.

o 암호에 내재 된 보안 취약점에도 불구하고 서버는 암호 인증을 지원해야합니다.

o 타사 응용 프로그램은 리소스 소유자의 보호 된 리소스에 지나치게 광범위하게 액세스 할 수 있으므로 리소스 소유자는 기간을 제한하거나 제한된 리소스 하위 집합에 대한 액세스 권한을 갖지 못합니다.

o 리소스 소유자는 모든 제 3 자에 대한 액세스를 취소하지 않고 개별 제 3 자에 대한 액세스를 취소 할 수 없으며 제 3 자의 비밀번호를 변경하여이를 취소해야합니다.

o 타사 응용 프로그램이 손상되면 최종 사용자의 암호와 해당 암호로 보호되는 모든 데이터가 손상됩니다.

OAuth는 권한 부여 계층을 도입하고 리소스 소유자의 역할과 클라이언트의 역할을 분리하여 이러한 문제를 해결합니다. OAuth에서 클라이언트는 리소스 소유자가 제어하고 리소스 서버에서 리소스에 대한 액세스를 요청하고 리소스 소유자와 다른 자격 증명 집합을 발급받습니다.

리소스 소유자의 자격 증명을 사용하여 보호 된 리소스에 액세스하는 대신 클라이언트는 특정 범위, 수명 및 기타 액세스 속성을 나타내는 문자열 인 액세스 토큰을 얻습니다. 액세스 토큰은 리소스 소유자의 승인을 받아 권한 부여 서버에서 타사 클라이언트에 발급됩니다. 클라이언트는 액세스 토큰을 사용하여 리소스 서버에서 호스팅하는 보호 된 리소스에 액세스합니다.

예를 들어, 최종 사용자 (자원 소유자)는 사용자 이름과 암호를 인쇄 서비스와 공유하지 않고 사진 공유 서비스 (자원 서버)에 저장된 보호 된 사진에 대한 인쇄 서비스 (클라이언트) 액세스 권한을 부여 할 수 있습니다. 대신 그녀는 인쇄 서비스 위임 특정 자격 증명 (액세스 토큰)을 발급하는 사진 공유 서비스 (인증 서버)에서 신뢰하는 서버로 직접 인증합니다.

이 사양은 HTTP ([[RFC2616](https://tools.ietf.org/html/rfc2616)])와 함께 사용하도록 설계되었습니다. HTTP 이외의 프로토콜을 통한 OAuth 사용은 범위를 벗어납니다.

정보 문서로 게시 된 OAuth 1.0 프로토콜 ([[RFC5849](https://tools.ietf.org/html/rfc5849)])은 소규모 임시 커뮤니티 노력의 결과였습니다. 이 표준 트랙 사양은 OAuth 1.0 배포 경험뿐만 아니라 광범위한 IETF 커뮤니티에서 수집 한 추가 사용 사례 및 확장 성 요구 사항을 기반으로합니다. OAuth 2.0 프로토콜은 OAuth 1.0과 역 호환되지 않습니다. 두 버전이 네트워크에 공존 할 수 있으며 구현시 둘 다 지원하도록 선택할 수 있습니다.그러나 새로운 구현은 이 문서에 지정된대로 OAuth 2.0을 지원하며 OAuth 1.0은 기존 배포를 지원하는 데만 사용됩니다. OAuth 2.0 프로토콜은 OAuth 1.0 프로토콜과 구현 세부 사항을 거의 공유하지 않습니다. OAuth 1.0에 익숙한 구현자는 구조와 세부 사항에 대한 추측없이 이 문서에 접근해야합니다.

### 1.1 Roles

OAuth는 네 가지 역할을 정의합니다.:

리소스(자원) 소유자

보호 된 리소스에 대한 액세스 권한을 부여 할 수있는 엔티티입니다. 리소스 소유자가 개인 인 경우이를 최종 사용자라고합니다.

리소스(자원) 서버

액세스 토큰을 사용하여 보호 된 리소스 요청을 수락하고 응답 할 수있는 보호 된 리소스를 호스팅하는 서버입니다..

클라이언트

리소스 소유자를 대신하여 권한을 부여하여 보호 된 리소스를 요청하는 애플리케이션입니다. "클라이언트"라는 용어는 특정 구현 특성 (예 : 응용 프로그램이 서버, 데스크톱 또는 기타 장치에서 실행되는지 여부)을 의미하지 않습니다.

권한 부여 서버

리소스 소유자를 성공적으로 인증하고 권한을 얻은 후 서버가 클라이언트에 액세스 토큰을 발급합니다.

권한 부여 서버와 리소스 서버 간의 상호 작용은 이 사양의 범위를 벗어납니다. 권한 서버는 자원 서버와 동일한 서버이거나 별도의 엔티티 일 수 있습니다. 단일 인증 서버는 여러 리소스 서버에서 허용하는 액세스 토큰을 발급 할 수 있습니다.

### 1.2 Protocol Flow

     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+

                     Figure 1: Abstract Protocol Flow

그림 1에 설명 된 OAuth 2.0 흐름은 네 가지 역할 간의 상호 작용을 설명하며 다음 단계를 포함합니다.:

(A) 클라이언트는 리소스 소유자에게 권한 부여를 요청합니다. 권한 부여 요청은 리소스 소유자 (표시된대로)에게 직접 만들어 지거나 중개자로서 권한 부여 서버를 통해 간접적으로 할 수 있습니다.

(B) 클라이언트는이 사양에 정의 된 네 가지 권한 부여 유형 중 하나를 사용하거나 확장 권한 부여 유형을 사용하여 표현 된 리소스 소유자의 권한을 나타내는 자격 증명인 권한 부여를받습니다. 권한 부여 유형은 클라이언트가 권한을 요청하는 데 사용하는 방법과 권한 부여 서버에서 지원하는 유형에 따라 다릅니다.

(C) 클라이언트는 권한 부여 서버로 인증하고 권한 부여를 제시하여 액세스 토큰을 요청합니다.

(D) 권한 부여 서버는 클라이언트를 인증하고 권한 부여를 확인하고 유효한 경우 액세스 토큰을 발급합니다.

(E) 클라이언트는 리소스 서버에서 보호 된 리소스를 요청하고 액세스 토큰을 제공하여 인증합니다.

(F) 리소스 서버는 액세스 토큰의 유효성을 검사하고 유효한 경우 요청을 처리합니다.

클라이언트가 리소스 소유자 (단계 (A) 및 (B)에 설명 됨)로부터 권한 부여를 얻기 위해 선호되는 방법은 권한 부여 서버를 중개자로 사용하는 것입니다. 이는 [Section 4.1](#41-Authorization-Code-Grant)의 그림 3에 설명되어 있습니다.

### 1.3 권한 부여

권한 부여는 클라이언트가 액세스 토큰을 얻기 위해 사용하는 리소스 소유자의 권한 (보호 된 리소스에 액세스하기위한)을 나타내는 자격 증명입니다. 이 사양은 권한 부여 코드, 암시, 리소스 소유자 암호 자격 증명 및 클라이언트 자격 증명의 네 가지 권한 부여 유형과 추가 유형을 정의하기위한 확장 성 메커니즘을 정의합니다.

#### 1.3.1. 권한 부여 코드

권한 부여 코드는 클라이언트와 리소스 소유자 사이의 중개자로 권한 서버를 사용하여 얻습니다. 리소스 소유자에게 직접 권한을 요청하는 대신 클라이언트는 리소스 소유자를 권한 부여 서버 ([[RFC2616](https://tools.ietf.org/html/rfc2616)]에 정의 된대로 사용자 에이전트를 통해)로 보내고, 다시 권한 부여 코드를 사용하여 리소스 소유자를 클라이언트로 다시 보냅니다.

권한 코드를 사용하여 리소스 소유자를 클라이언트로 다시 보내기 전에 권한 서버는 리소스 소유자를 인증하고 권한을 얻습니다. 리소스 소유자는 권한 부여 서버에서만 인증하기 때문에 리소스 소유자의 자격 증명은 클라이언트와 공유되지 않습니다.

권한 부여 코드는 클라이언트를 인증하는 기능과 같은 몇 가지 중요한 보안 이점을 제공 할뿐만 아니라 액세스 토큰을 리소스 소유자의 사용자 에이전트를 통해 전달하지 않고 클라이언트로 직접 전송하여 리소스 소유자를 포함하여 다른 이에게 노출시킬 수 있습니다.

#### 1.3.2. 암시적

암시적 권한 부여는 JavaScript와 같은 스크립팅 언어를 사용하여 브라우저에서 구현 된 클라이언트에 최적화 된 단순화 된 인증 코드 흐름입니다. 암시 적 흐름에서 클라이언트에게 권한 부여 코드를 발급하는 대신 클라이언트에 직접 액세스 토큰이 발급됩니다 (리소스 소유자 권한 부여의 결과로). 권한 부여 코드와 같은 중간 자격 증명이 발급되지 않고 나중에 액세스 토큰을 얻는 데 사용되기 때문에 부여 유형은 암시 적입니다.

암시적 허용 흐름 중에 액세스 토큰을 발행 할 때 권한 부여 서버는 클라이언트를 인증하지 않습니다. 경우에 따라 클라이언트에 액세스 토큰을 전달하는 데 사용되는 리디렉션 URI를 통해 클라이언트 정체를 확인할 수 있습니다. 액세스 토큰은 리소스 소유자 또는 리소스 소유자의 사용자 에이전트에 대한 액세스 권한이있는 다른 애플리케이션에 노출 될 수 있습니다.

암시적 권한 부여는 액세스 토큰을 얻는 데 필요한 왕복 횟수를 줄이므로 일부 클라이언트 (예 : 브라우저 내 애플리케이션으로 구현 된 클라이언트)의 응답 성과 효율성을 향상시킵니다. 그러나 이러한 편의는 특히 권한 부여 코드 부여 유형을 사용할 수있는 경우 섹션 [10.3](#103-Access-Tokens) 및 [10.16](#1016-Misuse-of-Access-Token-to-Impersonate-Resource-Owner-in-Implicit-Flow)에 설명 된 것과 같은 암시적 부여 사용의 보안에 대해 생각해보아야 합니다.

#### 1.3.3. 리소스 소유자 암호 자격 증명

리소스 소유자 암호 자격 증명 (즉, 사용자 이름 및 암호)은 액세스 토큰을 얻기위한 권한 부여로 직접 사용할 수 있습니다. 자격 증명은 리소스 소유자와 클라이언트간에 높은 수준의 신뢰가있을 때 (예 : 클라이언트가 장치 운영 체제 또는 높은 권한을 가진 응용 프로그램의 일부 임) 및 기타 권한 부여 유형을 사용할 수없는 경우에만 사용해야합니다 ( 권한 부여 코드 등).

이 권한 부여 유형에는 리소스 소유자 자격 증명에 대한 직접적인 클라이언트 액세스가 필요하지만 리소스 소유자 자격 증명은 단일 요청에 사용되며 액세스 토큰으로 교환됩니다. 이 권한 부여 유형은 자격 증명을 수명이 긴 액세스 토큰 또는 새로 고침 토큰으로 교환하여 클라이언트가 나중에 사용할 수 있도록 리소스 소유자 자격 증명을 저장할 필요를 제거 할 수 있습니다.

#### 1.3.4. 클라이언트 자격 증명

클라이언트 자격 증명 (또는 다른 형태의 클라이언트 인증)은 권한 부여 범위가 클라이언트의 제어하에있는 보호 된 리소스 또는 이전에 권한 부여 서버와 함께 준비된 보호된 리소스로 제한되는 경우 권한 부여로 사용될 수 있습니다. 클라이언트 자격 증명은 일반적으로 클라이언트가 자체적으로 작업하거나 (클라이언트가 리소스 소유자이기도 함) 이전에 권한 부여 서버에 준비된 권한을 기반으로 보호 된 리소스에 대한 액세스를 요청할 때 권한 부여로 사용됩니다.

### 1.4. 액세스 토큰

액세스 토큰은 보호 된 리소스에 액세스하는 데 사용되는 자격 증명입니다. 액세스 토큰은 클라이언트에 발급 된 권한을 나타내는 문자열입니다. 문자열은 일반적으로 클라이언트에게 불명료한 값 입니다. 토큰은 리소스 소유자가 부여하고 리소스 서버 및 권한 부여 서버에 의해 시행되는 특정 범위 및 액세스 기간을 나타냅니다.

토큰은 인증 정보를 검색하는 데 사용되는 식별자를 나타내거나 검증 가능한 방식으로 인증 정보를 자체 포함 할 수 있습니다 (즉, 일부 데이터와 서명으로 구성된 토큰 문자열). 클라이언트가 토큰을 사용하려면이 사양의 범위를 벗어나는 추가 인증 자격 증명이 필요할 수 있습니다.

액세스 토큰은 추상화 계층을 제공하여 서로 다른 인증 구성 (예 : 사용자 이름 및 암호)을 리소스 서버가 이해하는 단일 토큰으로 대체합니다. 이 추상화를 통해 액세스 토큰을 얻는 데 사용되는 권한 부여보다 더 제한적인 액세스 토큰을 발급 할 수있을뿐만 아니라 광범위한 인증 방법을 이해해야하는 리소스 서버의 필요성을 제거 할 수 있습니다..

액세스 토큰은 리소스 서버 보안 요구 사항에 따라 다양한 형식, 구조 및 활용 방법 (예 : 암호화 속성)을 가질 수 있습니다. 보호 된 리소스에 액세스하는 데 사용되는 액세스 토큰 속성 및 방법은이 사양의 범위를 벗어나며 [[RFC6750](https://tools.ietf.org/html/rfc6750)]과 같은 동반 사양에 의해 정의됩니다.

### 1.5. 새로 고침 토큰

새로 고침 토큰은 액세스 토큰을 얻는 데 사용되는 자격 증명입니다. 새로 고침 토큰은 권한 부여 서버에 의해 클라이언트에 발급되며 현재 액세스 토큰이 유효하지 않거나 만료 될 때 새 액세스 토큰을 얻거나 범위가 동일하거나 더 좁은 추가 액세스 토큰을 얻는 데 사용됩니다 (액세스 토큰의 수명이 짧을 수 있으며 리소스 소유자가 승인 한 것보다 적은 권한). 새로 고침 토큰 발급은 권한 부여 서버의 재량에 따라 선택 사항입니다. 인증 서버가 새로 고침 토큰을 발행하면 액세스 토큰을 발행 할 때 포함됩니다 (예 : 그림 1의 단계 (D)).

새로 고침 토큰은 리소스 소유자가 클라이언트에 부여한 권한을 나타내는 문자열입니다. 문자열은 일반적으로 클라이언트에게 불명료한 값입니다. 토큰은 인증 정보를 검색하는 데 사용되는 식별자를 나타냅니다. 액세스 토큰과 달리 새로 고침 토큰은 권한 부여 서버에서만 사용하기위한 것이며 리소스 서버로 전송되지 않습니다.

     +--------+                                           +---------------+
     |        |--(A)------- Authorization Grant --------->|               |
     |        |                                           |               |
     |        |<-(B)----------- Access Token -------------|               |
     |        |               & Refresh Token             |               |
     |        |                                           |               |
     |        |                            +----------+   |               |
     |        |--(C)---- Access Token ---->|          |   |               |
     |        |                            |          |   |               |
     |        |<-(D)- Protected Resource --| Resource |   | Authorization |
     | Client |                            |  Server  |   |     Server    |
     |        |--(E)---- Access Token ---->|          |   |               |
     |        |                            |          |   |               |
     |        |<-(F)- Invalid Token Error -|          |   |               |
     |        |                            +----------+   |               |
     |        |                                           |               |
     |        |--(G)----------- Refresh Token ----------->|               |
     |        |                                           |               |
     |        |<-(H)----------- Access Token -------------|               |
     +--------+           & Optional Refresh Token        +---------------+
                  Figure 2: Refreshing an Expired Access Token

그림 2에 표시된 흐름에는 다음 단계가 포함됩니다.:

(A) 클라이언트는 권한 서버로 인증하고 권한 부여를 제시하여 액세스 토큰을 요청합니다.

(B) 권한 부여 서버는 클라이언트를 인증하고 권한 부여의 유효성을 검사하고 유효한 경우 액세스 토큰과 새로 고침 토큰을 발급합니다.

(C) 클라이언트는 액세스 토큰을 제공하여 보호된 리소스를 리소스 서버에 요청합니다.

(D) 리소스 서버는 액세스 토큰의 유효성을 검사하고 유효한 경우 요청을 처리합니다.

(E) 액세스 토큰이 만료 될 때까지 (C) 및 (D) 단계를 반복합니다. 클라이언트가 액세스 토큰이 만료되었음을 알고 있으면 단계 (G)로 건너 뜁니다. 그렇지 않으면 다른 보호 된 리소스 요청을합니다.

(F) 액세스 토큰이 유효하지 않기 때문에 리소스 서버가 유효하지 않은 토큰 오류를 반환합니다.

(G) 클라이언트는 권한 부여 서버로 인증하고 새로 고침 토큰을 제공하여 새 액세스 토큰을 요청합니다. 클라이언트 인증 요구 사항은 클라이언트 유형 및 권한 서버 정책을 기반으로합니다.

(H) 권한 부여 서버는 클라이언트를 인증하고 새로 고침 토큰의 유효성을 검사하고 유효한 경우 새 액세스 토큰 (및 선택적으로 새 새로 고침 토큰)을 발급합니다.

단계 (C), (D), (E) 및 (F)는 [Section 7](#7-Accessing-Protected-Resources)에 설명 된대로이 사양의 범위를 벗어납니다.

### 1.6. TLS Version

이 사양에서 TLS (전송 계층 보안)를 사용할 때마다 TLS의 적절한 버전 (또는 버전)은 광범위한 배포 및 알려진 보안 취약성에 따라 시간이 지남에 따라 달라집니다. 이 글을 쓰는 시점에서 TLS 버전 1.2 [[RFC5246](https://tools.ietf.org/html/rfc5246)]가 가장 최신 버전이지만 배포 기반이 매우 제한적이며 구현에 쉽게 사용할 수 없습니다. TLS 버전 1.0 [[RFC2246](https://tools.ietf.org/html/rfc2246)]은 가장 널리 배포 된 버전이며 가장 광범위한 상호 운용성을 제공합니다.

구현시 보안 요구 사항을 충족하는 추가 전송 계층 보안 메커니즘을 지원할 수도 있습니다.(MAY)

### 1.7. HTTP Redirections

이 사양은 클라이언트 또는 권한 부여 서버가 리소스 소유자의 사용자 에이전트를 다른 대상으로 보내는 HTTP 리디렉션을 광범위하게 사용합니다. 이 사양의 예제는 HTTP 302 상태 코드의 사용을 보여 주지만,이 리디렉션을 수행하기 위해 사용자 에이전트를 통해 사용할 수있는 다른 모든 방법이 허용되며 구현 세부 사항으로 간주됩니다.

### 1.8. 상호 운용성

OAuth 2.0은 잘 정의 된 보안 속성과 함께 풍부한 권한 부여 프레임 워크를 제공합니다. 그러나 많은 선택적 구성 요소가있는 풍부하고 확장 성이 뛰어난 프레임 워크로서 이 사양은 상호 운용이 불가능한 광범위한 구현을 생성 할 가능성이 높습니다.

또한이 사양은 일부 필수 구성 요소를 부분적으로 또는 완전히 정의하지 않은 상태로 둡니다 (예 : 클라이언트 등록, 권한 부여 서버 기능, 엔드 포인트 검색). 이러한 구성 요소가없는 경우 클라이언트는 상호 운용하기 위해 특정 권한 서버 및 자원 서버에 대해 수동으로 그리고 구체적으로 구성되어야합니다.

이 프레임 워크는 향후 작업이 완전한 웹 규모 상호 운용성을 달성하는 데 필요한 규범 적 프로필 및 확장을 정의 할 것이라는 명확한 기대를 바탕으로 설계되었습니다.

### 1.9. Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
specification are to be interpreted as described in [[RFC2119](https://tools.ietf.org/html/rfc2119)].

This specification uses the Augmented Backus-Naur Form (ABNF)
notation of [[RFC5234](https://tools.ietf.org/html/rfc5234)]. Additionally, the rule URI-reference is
included from "Uniform Resource Identifier (URI): Generic Syntax"
[[RFC3986](https://tools.ietf.org/html/rfc3986)].

Certain security-related terms are to be understood in the sense
defined in [[RFC4949](https://tools.ietf.org/html/rfc4949)]. These terms include, but are not limited to,
"attack", "authentication", "authorization", "certificate",
"confidentiality", "credential", "encryption", "identity", "sign",
"signature", "trust", "validate", and "verify".

Unless otherwise noted, all the protocol parameter names and values
are case sensitive.

## 2. 클라이언트 등록

프로토콜을 시작하기 전에 클라이언트는 인증 서버에 등록합니다. 클라이언트가 인증 서버에 등록하는 수단은 이 사양의 범위를 벗어나지 만 일반적으로 최종 사용자와 HTML 등록 양식과의 상호 작용을 포함합니다.

클라이언트 등록에는 클라이언트와 권한 부여 서버 간의 직접적인 상호 작용이 필요하지 않습니다. 권한 부여 서버에서 지원하는 경우 등록은 클라이언트와 신뢰를 설정하고 필요한 클라이언트 속성 (예 : 리디렉션 URI, 클라이언트 유형)을 얻기위한 다른 수단에 의존 할 수 있습니다. 예를 들어, 등록은 자체 발행 또는 제 3 자 발행을 사용하거나 신뢰할 수있는 채널을 사용하여 클라이언트 발견을 수행하는 권한 부여 서버에 의해 수행 될 수 있습니다. 클라이언트를 등록 할 때 클라이언트 개발자는 다음을 해야한다.(SHALL):

o [Section 2.1](#21-Client-Types)에 설명 된대로 클라이언트 유형을 지정합니다.

o [Section 3.1.2](#312-Redirection-Endpoint)에 설명 된대로 클라이언트 리디렉션 URI를 제공합니다.

o 권한 부여 서버에 필요한 기타 정보 (예 : 애플리케이션 이름, 웹 사이트, 설명, 로고 이미지, 법적 조건 수락)를 포함합니다.

### 2.1. 클라이언트 유형

OAuth는 권한 부여 서버로 안전하게 인증하는 능력 (즉, 클라이언트 자격 증명의 기밀성을 유지하는 능력)에 따라 두 가지 클라이언트 유형을 정의합니다.:

confidential(기밀)

자격 증명의 기밀성을 유지할 수있는 클라이언트 (예 : 클라이언트 자격 증명에 대한 액세스가 제한된 보안 서버에 구현 된 클라이언트) 또는 다른 수단을 사용하여 클라이언트 인증을 보호 할 수 있습니다.

public(공용)

자격 증명의 기밀성을 유지할 수없는 클라이언트 (예 : 설치된 기본 응용 프로그램 또는 웹 브라우저 기반 응용 프로그램과 같이 리소스 소유자가 사용하는 장치에서 실행되는 클라이언트), 다른 수단을 통한 보안 클라이언트 인증이 불가능합니다.

클라이언트 유형 지정은 권한 부여 서버의 보안 인증 정의와 클라이언트 자격 증명의 허용 가능한 노출 수준을 기반으로합니다. 권한 부여 서버는 클라이언트 유형에 대해 가정하지 않아야합니다.(SHOULD NOT)

클라이언트는 서로 다른 클라이언트 유형 및 보안 컨텍스트 (예 : 기밀 서버 기반 구성 요소와 공용 브라우저 기반 구성 요소를 모두 포함하는 분산 클라이언트)를 가진 분산 된 구성 요소 집합으로 구현 될 수 있습니다. 인증 서버가 이러한 클라이언트에 대한 지원을 제공하지 않거나 등록에 대한 지침을 제공하지 않는 경우 클라이언트는 각 구성 요소를 별도의 클라이언트로 등록해야합니다.(SHOULD)

이 사양은 다음 클라이언트 프로필을 중심으로 설계되었습니다.:

웹 애플리케이션

웹 애플리케이션은 웹 서버에서 실행되는 기밀 클라이언트입니다. 리소스 소유자는 리소스 소유자가 사용하는 장치의 사용자 에이전트에서 렌더링 된 HTML 사용자 인터페이스를 통해 클라이언트에 액세스합니다. 클라이언트 자격 증명 및 클라이언트에 발급 된 모든 액세스 토큰은 웹 서버에 저장되며 리소스 소유자에게 노출되거나 액세스 할 수 없습니다.

사용자 에이전트 기반 애플리케이션

사용자 에이전트 기반 애플리케이션은 클라이언트 코드가 웹 서버에서 다운로드되고 리소스 소유자가 사용하는 장치의 사용자 에이전트 (예 : 웹 브라우저) 내에서 실행되는 공용 클라이언트입니다. 프로토콜 데이터 및 자격 증명은 리소스 소유자가 쉽게 액세스 할 수 있으며 종종 볼 수 있습니다. 이러한 애플리케이션은 사용자 에이전트 내에 상주하므로 권한 부여를 요청할 때 사용자 에이전트 기능을 원활하게 사용할 수 있습니다.

기본 응용 프로그램

기본 애플리케이션은 리소스 소유자가 사용하는 장치에 설치 및 실행되는 공용 클라이언트입니다. 리소스 소유자는 프로토콜 데이터 및 자격 증명에 액세스 할 수 있습니다. 응용 프로그램에 포함 된 모든 클라이언트 인증 자격 증명을 추출 할 수 있다고 가정합니다. 반면에 액세스 토큰 또는 새로 고침 토큰과 같이 동적으로 발급 된 자격 증명은 허용 가능한 수준의 보호를받을 수 있습니다. 최소한 이러한 자격 증명은 애플리케이션이 상호 작용할 수있는 적대적인 서버로부터 보호됩니다. 일부 플랫폼에서는 이러한 자격 증명이 동일한 장치에있는 다른 응용 프로그램으로부터 보호 될 수 있습니다.

### 2.2. 클라이언트 식별

권한 부여 서버는 등록 된 클라이언트에게 클라이언트 식별자 (클라이언트가 제공 한 등록 정보를 나타내는 고유 한 문자열)를 발급합니다. 클라이언트 식별자는 비밀이 아닙니다. 리소스 소유자에게 노출되며 클라이언트 인증을 위해 단독으로 사용해서는 안됩니다(MUST NOT). 클라이언트 식별자는 권한 부여 서버에 고유합니다.

클라이언트 식별자 문자열 크기는이 사양에 정의되지 않은 상태로 남아 있습니다. 클라이언트는 식별자 크기에 대한 가정을 피해야합니다. 권한 부여 서버는 발행하는 식별자의 크기를 문서화해야합니다(SHOULD).

### 2.3. 클라이언트 인증

클라이언트 유형이 기밀 인 경우 클라이언트와 권한 서버는 권한 부여 서버의 보안 요구 사항에 적합한 클라이언트 인증 방법을 설정합니다. 권한 부여 서버는 보안 요구 사항을 충족하는 모든 형태의 클라이언트 인증을 수락 할 수 있습니다(MAY).

기밀 클라이언트는 일반적으로 권한 부여 서버 (예 : 암호, 공개 / 개인 키 쌍)로 인증하는 데 사용되는 클라이언트 자격 증명 집합을 발급 (또는 설정)합니다.

권한 부여 서버는 공용 클라이언트와 클라이언트 인증 방법을 설정할 수 있습니다(MAY). 그러나 권한 부여 서버는 클라이언트 식별 목적으로 공용 클라이언트 인증에 의존해서는 안됩니다(MUST NOT).

클라이언트는 각 요청에서 둘 이상의 인증 방법을 사용해서는 안됩니다(MUST NOT).

#### 2.3.1. 클라이언트 비밀번호

클라이언트 암호를 소유 한 클라이언트는 [[RFC2617](https://tools.ietf.org/html/rfc2617)]에 정의 된 HTTP 기본 인증 체계를 사용하여 권한 부여 서버로 인증 할 수 있습니다. 클라이언트 식별자는 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라 "application / x-www-form-urlencoded"인코딩 알고리즘을 사용하여 인코딩되며 인코딩 된 값이 사용자 이름으로 사용됩니다. 클라이언트 암호는 동일한 알고리즘을 사용하여 인코딩되고 암호로 사용됩니다. 권한 부여 서버는 클라이언트 암호를 발급 한 클라이언트를 인증하기 위해 HTTP 기본 인증 체계를 지원해야합니다.

예를 들어 (표시 목적으로 만 추가 줄 바꿈 포함):

     Authorization: Basic czZCaGRSa3F0Mzo3RmpmcDBaQnIxS3REUmJuZlZkbUl3

또는 권한 부여 서버는 다음 매개 변수를 사용하여 요청 본문에 클라이언트 자격 증명을 포함하는 것을 지원할 수 있습니다.:

client_id

REQUIRED. [Section 2.2](#22-Client-Identifier)에 설명 된 등록 프로세스 중에 클라이언트에게 발급 된 클라이언트 식별자.

client_secret

REQUIRED. 클라이언트 시크릿. 클라이언트 시크릿이 빈 문자열 인 경우 클라이언트는 매개 변수를 생략 할 수 있습니다.

두 매개 변수를 사용하여 요청 본문에 클라이언트 자격 증명을 포함하는 것은 권장되지 않으며 HTTP 기본 인증 체계 (또는 기타 암호 기반 HTTP 인증 체계)를 직접 사용할 수없는 클라이언트로 제한되어야합니다. 매개 변수는 요청 본문에서만 전송 될 수 있으며 요청 URI에 포함되지 않아야합니다.

예를 들어 본문 매개 변수를 사용하여 액세스 토큰 ([Section 6](#6-Refreshing-an-Access-Token))을 새로 고치는 요청 (표시 목적으로 만 추가 줄 바꿈 포함):

     POST /token HTTP/1.1
     Host: server.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
     &client_id=s6BhdRkqt3&client_secret=7Fjfp0ZBr1KtDRbnfVdmIw

권한 부여 서버는 암호 인증을 사용하여 요청을 보낼 때 [Section 1.6](#16-TLS-Version)에 설명 된대로 TLS를 사용해야합니다.

이 클라이언트 인증 방법에는 암호가 포함되므로 권한 부여 서버는 무차별 대입 공격으로부터 암호를 사용하는 모든 엔드 포인트를 보호해야합니다.

#### 2.3.2. 기타 인증 방법

권한 부여 서버는 보안 요구 사항과 일치하는 적절한 HTTP 인증 체계를 지원할 수 있습니다. 다른 인증 방법을 사용할 때 권한 부여 서버는 클라이언트 식별자 (등록 레코드)와 인증 체계 간의 매핑을 정의해야합니다.

### 2.4. 등록되지 않은 클라이언트

이 사양은 등록되지 않은 클라이언트의 사용을 제외하지 않습니다. 그러나 이러한 클라이언트의 사용은이 사양의 범위를 벗어나며 추가 보안 분석 및 상호 운용성 영향 검토가 필요합니다.

## 3. Protocol Endpoints

권한 부여 프로세스는 두 개의 권한 부여 서버 endpoints (HTTP 리소스)을 사용합니다.:

o 권한 부여 endpoint - 사용자 에이전트 리디렉션을 통해 리소스 소유자로부터 권한을 얻기 위해 클라이언트가 사용합니다.

o 토큰 endpoint - 클라이언트가 일반적으로 클라이언트 인증을 사용하여 액세스 토큰에 대한 권한 부여를 교환하는 데 사용됩니다.

o 리디렉션 endpoint - 권한 부여 서버가 리소스 소유자 사용자 에이전트를 통해 권한 자격 증명이 포함 된 응답을 클라이언트에 반환하는 데 사용됩니다.

모든 권한 부여 유형이 두 Endpoint을 모두 사용하는 것은 아닙니다. 확장 부여 유형은 필요에 따라 추가 엔드 포인트를 정의 할 수 있습니다.

### 3.1. 권한 부여 Endpoint

권한 부여 endpoint은 리소스 소유자와 상호 작용하고 권한 부여를 얻는 데 사용됩니다. 권한 부여 서버는 먼저 자원 소유자의 신원을 확인해야합니다. 권한 부여 서버가 리소스 소유자를 인증하는 방식 (예 : 사용자 이름 및 비밀번호 로그인, 세션 쿠키)은이 사양의 범위를 벗어납니다.

클라이언트가 권한 부여 endpoint의 위치를 ​​얻는 수단은이 사양의 범위를 벗어나지 만 위치는 일반적으로 서비스 설명서에 제공됩니다.

endpoint URI는 추가 쿼리 매개 변수를 추가 할 때 유지되어야하는 (부록 B에 따라) 쿼리 구성 요소 ([[RFC3986] Section 3.4](https://tools.ietf.org/html/rfc3986#section-3.4)) 형식의 "application / x-www-form-urlencoded"를 포함 할 수 있습니다. endpoint URI는 fragment 요소를 포함하지 않아야합니다.

권한 부여 endpoint에 대한 요청은 사용자 인증과 일반 텍스트 자격 증명 (HTTP 응답에서) 전송을 가져 오므로 권한 부여 서버는 권한 부여 endpoint에 요청을 보낼 때 [Section 1.6](#16-TLS-Version)에 설명 된대로 TLS를 사용해야합니다.

권한 부여 서버는 인증 endpoint에 대해 HTTP "GET"메소드 [[RFC2616](https://tools.ietf.org/html/rfc2616)]의 사용을 지원해야하며 "POST"메소드의 사용도 지원할 수 있습니다.

값없이 전송 된 매개 변수는 요청에서 생략 된 것처럼 처리되어야합니다. 권한 부여 서버는 인식되지 않는 요청 매개 변수를 무시해야합니다. 요청 및 응답 매개 변수는 두 번 이상 포함되지 않아야합니다.

#### 3.1.1. 응답 유형

권한 부여 endpoint은 권한 부여 코드 부여 유형 및 암시 적 부여 유형 흐름에서 사용됩니다. 클라이언트는 다음 매개 변수를 사용하여 원하는 권한 부여 유형을 권한 부여 서버에 알립니다.:

response_type

REQUIRED. 값은 [Section 4.1.1](#411-Authorization-Request)에 설명 된대로 권한 부여 코드를 요청하기위한 "code", [Section 4.2.1](#421-Authorization-Request)에 설명 된 액세스 토큰 (암시적 허용)을 요청하기위한 "token"또는 [Section 8.4](#8..Defining-New-Authorization-Endpoint-Response-Types)에 설명 된 등록 된 확장 값 중 하나 여야합니다.

확장 응답 유형은 값의 순서가 중요하지 않은 공백으로 구분 된 (%x20) 값 목록을 포함 할 수 있습니다 (예 : 응답 유형 "a b"는 "b a"와 동일 함). 이러한 복합 응답 유형의 의미는 해당 사양에 의해 정의됩니다.

인증 요청에 "response_type"매개 변수가 없거나 응답 유형이 이해되지 않는 경우 권한 부여 서버는 [Section 4.1.2.1](#4.1.2.1.-Error-Response)에 설명 된대로 오류 응답을 반환해야합니다.

#### 3.1.2. 리디렉션 Endpoint

자원 소유자와의 상호 작용을 완료 한 후 권한 부여 서버는 자원 소유자의 사용자 에이전트를 다시 클라이언트로 보냅니다. 권한 부여 서버는 클라이언트 등록 프로세스 중 또는 권한 부여 요청을 할 때 이전에 권한 부여 서버에 설정된 클라이언트의 리디렉션 endpoint으로 사용자 에이전트를 리디렉션합니다.

리디렉션 endpoint URI는 [[RFC3986] Section 4.3](https://tools.ietf.org/html/rfc3986#section-4.3)에 정의 된대로 절대 URI 여야합니다. endpoint URI는 추가 쿼리 매개 변수를 추가 할 때 유지되어야하는 ([Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라) 쿼리 구성 요소 ([[RFC3986] Section 3.4](https://tools.ietf.org/html/rfc3986#section-3.4)) 형식의 "application/x-www-form-urlencoded"를 포함 할 수 있습니다. Endpoint URI는 fragment 요소를 포함하지 않아야합니다.

##### 3.1.2.1. Endpoint 요청 기밀성

리디렉션 endpoint는 요청 된 응답 유형이 "code"또는 "token"이거나 리디렉션 요청이 개방형 네트워크를 통해 민감한 자격 증명을 전송하게 될 때 [Section 1.6](#16-TLS-Version)에 설명 된대로 TLS를 사용해야합니다 (SHOULD). 이 문서를 작성할 당시 클라이언트에게 TLS를 배포하도록 요구하는 것은 많은 클라이언트 개발자에게 중요한 장애물이기 때문에이 사양은 TLS 사용을 의무화하지 않습니다. TLS를 사용할 수없는 경우 권한 부여 서버는 리디렉션 전에 안전하지 않은 endpoint에 대해 리소스 소유자에게 경고해야합니다 (예 : 권한 부여 요청 중에 메시지 표시).

전송 계층 보안이 부족하면 클라이언트 및 액세스 권한이있는 보호 된 리소스의 보안에 심각한 영향을 미칠 수 있습니다. 전송 계층 보안의 사용은 권한 부여 프로세스가 클라이언트에 의해 위임 된 최종 사용자 인증의 한 형태로 사용될 때 특히 중요합니다 (예 : 타사 로그인 서비스).

##### 3.1.2.2. 등록 요건

권한 부여 서버는 리디렉션 endpoint을 등록하기 위해 다음 클라이언트를 요구해야합니다.:

o 공용 클라이언트.

o 암시적 부여 유형을 사용하는 기밀 클라이언트.

권한 부여 서버는 모든 클라이언트가 권한 부여 endpoint을 사용하기 전에 리디렉션 endpoint을 등록하도록 요구해야합니다(SHOULD).

권한 부여 서버는 클라이언트가 완전한 리디렉션 URI를 제공하도록 요구해야합니다 (클라이언트는 요청 별 사용자 정의를 달성하기 위해 "state"요청 매개 변수를 사용할 수 있습니다). 완전한 리디렉션 URI의 등록을 요구할 수없는 경우 권한 부여 서버는 URI scheme, 권한 및 경로의 등록을 요구해야합니다 (권한을 요청할 때 클라이언트가 리디렉션 URI의 쿼리 구성 요소 만 동적으로 변경할 수 있도록 허용).

권한 부여 서버는 클라이언트가 여러 리디렉션 Endpoint을 등록하도록 허용 할 수 있습니다.

리디렉션 URI 등록 요구 사항이 없으면 공격자가 [Section 10.15](#1015-Open-Redirectors)에 설명 된대로 권한 부여 endpoint을 개방형 리디렉터로 사용할 수 있습니다.

##### 3.1.2.3. 동적 구성

여러 리디렉션 URI가 등록 된 경우, 리디렉션 URI의 일부만 등록되었거나 리디렉션 URI가 등록되지 않은 경우 클라이언트는 "redirect_uri"요청 매개 변수를 사용하는 인증 요청과 함께 리디렉션 URI를 포함해야합니다.

리디렉션 URI가 권한 요청에 포함 된 경우 권한 부여 서버는 리디렉션 URI가 등록 된 경우 [[RFC3986] Section 6](https://tools.ietf.org/html/rfc3986#section-6)에 정의 된 등록 된 리디렉션 URI (또는 URI 구성 요소) 중 하나 이상과 수신 된 값을 비교하고 일치시켜야합니다. . 클라이언트 등록에 전체 리디렉션 URI가 포함 된 경우 인증 서버는 [[RFC3986] Section 6.2.1.](https://tools.ietf.org/html/rfc3986#section-6.2.1)에 정의 된 간단한 문자열 비교를 사용하여 두 URI를 비교해야합니다.

##### 3.1.2.4. 무효한 Endpoint

권한 부여 요청이 누락되거나 유효하지 않거나 일치하지 않는 리디렉션 URI로 인해 유효성 검사에 실패하면 권한 부여 서버는 리소스 소유자에게 오류를 알려야하며 사용자 에이전트를 잘못된 리디렉션 URI로 자동 리디렉션해서는 안됩니다.

##### 3.1.2.5. Endpoint 콘텐츠

클라이언트의 Endpoint에 대한 리디렉션 요청은 일반적으로 사용자 에이전트가 처리하는 HTML 문서 응답을 생성합니다. HTML 응답이 리디렉션 요청의 결과로 직접 제공되는 경우 HTML 문서에 포함 된 모든 스크립트는 리디렉션 URI 및 포함 된 자격 증명에 대한 전체 액세스 권한으로 실행됩니다.

클라이언트는 리디렉션 엔드 포인트 응답에 타사 스크립트 (예 : 타사 분석, 소셜 플러그인, 광고 네트워크)를 포함하면 안됩니다. 대신 URI에서 자격 증명을 추출하고 자격 증명을 노출하지 않고 (URI 또는 ​​다른 곳에서) 사용자 에이전트를 다시 다른 Endpoint으로 리디렉션해야합니다 (SHOULD). 타사 스크립트가 포함 된 경우 클라이언트는 자신의 스크립트 (URI에서 자격 증명을 추출 및 제거하는 데 사용됨)가 먼저 실행되는지 확인해야합니다.

### 3.2. 토큰 Endpoint

토큰 endpoint은 클라이언트가 권한 부여 또는 새로 고침 토큰을 제공하여 액세스 토큰을 얻는 데 사용됩니다. 토큰 endpoint은 암시 적 권한 부여 유형을 제외한 모든 권한 부여와 함께 사용됩니다 (액세스 토큰이 직접 발급되기 때문에).

클라이언트가 토큰 endpoint의 위치를 ​​얻는 방법은이 사양의 범위를 벗어나지 만 일반적으로 위치는 서비스 설명서에 제공됩니다.

Endpoint URI는 추가 쿼리 매개 변수를 추가 할 때 유지되어야하는 ([Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라) 쿼리 구성 요소 ([[RFC3986] Section 3.4](https://tools.ietf.org/html/rfc3986#section-3.4)) 형식의 "application/x-www-form-urlencoded"를 포함 할 수 있습니다. Endpoint URI는 fragment 구성 요소를 포함하지 않아야합니다.

토큰 Endpoint에 대한 요청은 일반 텍스트 자격 증명 (HTTP 요청 및 응답)을 전송하므로 권한 부여 서버는 요청을 토큰 Endpoint에 보낼 때 [Section 1.6](#16-TLS-Version)에 설명 된대로 TLS를 사용해야합니다.

클라이언트는 액세스 토큰 요청을 할 때 반드시 HTTP "POST"메소드를 사용해야합니다.

값없이 전송 된 매개 변수는 요청에서 생략 된 것처럼 처리되어야합니다. 권한 부여 서버는 인식되지 않는 요청 매개 변수를 무시해야합니다. 요청 및 응답 매개 변수는 두 번 이상 포함되지 않아야합니다.

#### 3.2.1. 클라이언트 인증

기밀 클라이언트 또는 기타 클라이언트가 발급 한 클라이언트 자격 증명은 토큰 엔드 포인트에 요청할 때 [Section 2.3](#23-Client-Authentication) 절에 설명 된대로 인증 서버로 인증해야합니다. 클라이언트 인증은 다음에 사용됩니다.:

o 발급 된 클라이언트에 대한 새로 고침 토큰 및 권한 부여 코드의 바인딩을 적용합니다. 클라이언트 인증은 인증 코드가 안전하지 않은 채널을 통해 리디렉션 Endpoint로 전송되거나 리디렉션 URI가 완전히 등록되지 않은 경우에 중요합니다.

o 클라이언트를 비활성화하거나 자격 증명을 변경하여 손상된 클라이언트에서 복구하여 공격자가 훔친 새로 고침 토큰을 남용하지 못하도록합니다. 단일 클라이언트 자격 증명 집합을 변경하는 것이 전체 새로 고침 토큰 집합을 취소하는 것보다 훨씬 빠릅니다.

o 정기적 인 자격 증명 교체가 필요한 인증 관리 모범 사례를 구현합니다. 전체 새로 고침 토큰 집합을 교체하는 것은 어려울 수 있지만 단일 클라이언트 자격 증명 집합을 교체하는 것은 훨씬 쉽습니다.

클라이언트는 "client_id"요청 매개 변수를 사용하여 토큰 Endpoint에 요청을 보낼 때 자신을 식별 할 수 있습니다. 토큰 엔드 포인트에 대한 "authorization_code" "grant_type"요청에서 인증되지 않은 클라이언트는 "client_id"가 다른 "client_id"를 가진 클라이언트 용으로 의도 된 코드를 실수로 수락하는 것을 방지하기 위해 반드시 "client_id"를 전송해야합니다. 이것은 인증 코드의 대체로부터 클라이언트를 보호합니다. (보호 된 자원에 대한 추가 보안을 제공하지 않습니다.)

### 3.3. 액세스 토큰 범위

권한 부여 및 토큰 Endpoint를 통해 클라이언트는 "scope"요청 매개 변수를 사용하여 액세스 요청의 범위를 지정할 수 있습니다. 권한 부여 서버는 "scope"응답 매개 변수를 사용하여 발급 된 액세스 토큰의 범위를 클라이언트에 알립니다.

범위 매개 변수의 값은 공백으로 구분되고 대소 문자를 구분하는 문자열 목록으로 표현됩니다. 문자열은 권한 부여 서버에 의해 정의됩니다. 값에 공백으로 구분 된 여러 문자열이 포함 된 경우 순서는 중요하지 않으며 각 문자열은 요청 된 범위에 추가 액세스 범위를 추가합니다.

     scope       = scope-token *( SP scope-token )
     scope-token = 1*( %x21 / %x23-5B / %x5D-7E )

권한 서버는 권한 서버 정책 또는 리소스 소유자의 지시에 따라 클라이언트가 요청한 범위를 전체 또는 부분적으로 무시할 수 있습니다. 발급 된 액세스 토큰 범위가 클라이언트가 요청한 범위와 다른 경우 권한 부여 서버는 "scope"응답 매개 변수를 포함하여 클라이언트에게 부여 된 실제 범위를 알려야합니다.

클라이언트가 권한을 요청할 때 범위 매개 변수를 생략하면 권한 서버는 미리 정의 된 기본값을 사용하여 요청을 처리하거나 유효하지 않은 범위를 나타내는 요청을 실패해야합니다. 권한 부여 서버는 범위 요구 사항과 기본값 (정의 된 경우)을 문서화해야합니다 (SHOULD).

## 4. 권한 부여 받기

액세스 토큰을 요청하기 위해 클라이언트는 리소스 소유자로부터 권한을 얻습니다. 권한 부여는 클라이언트가 액세스 토큰을 요청하는 데 사용하는 권한 부여의 형태로 표현됩니다. OAuth는 인증 코드, 암시 적, 리소스 소유자 암호 자격 증명 및 클라이언트 자격 증명의 네 가지 부여 유형을 정의합니다. 또한 추가 부여 유형을 정의하기위한 확장 메커니즘을 제공합니다.

### 4.1. 인증 코드 부여

권한 부여 코드 유형은 액세스 토큰과 새로 고침 토큰을 모두 얻는 데 사용되며 기밀 클라이언트에 최적화되어 있습니다. 이것은 리디렉션 기반 흐름이므로 클라이언트는 리소스 소유자의 사용자 에이전트 (일반적으로 웹 브라우저)와 상호 작용할 수 있어야하며 권한 부여 서버에서 들어오는 요청 (리디렉션을 통해)을받을 수 있어야합니다.

     +----------+
     | Resource |
     |   Owner  |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier      +---------------+
     |         -+----(A)-- & Redirection URI ---->|               |
     |  User-   |                                 | Authorization |
     |  Agent  -+----(B)-- User authenticates --->|     Server    |
     |          |                                 |               |
     |         -+----(C)-- Authorization Code ---<|               |
     +-|----|---+                                 +---------------+
       |    |                                         ^      v
      (A)  (C)                                        |      |
       |    |                                         |      |
       ^    v                                         |      |
     +---------+                                      |      |
     |         |>---(D)-- Authorization Code ---------'      |
     |  Client |          & Redirection URI                  |
     |         |                                             |
     |         |<---(E)----- Access Token -------------------'
     +---------+       (w/ Optional Refresh Token)

      Note: The lines illustrating steps (A), (B), and (C) are broken into
      two parts as they pass through the user-agent.

                     Figure 3: Authorization Code Flow

그림 3에 표시된 흐름에는 다음 단계가 포함됩니다.

(A) 클라이언트는 리소스 소유자의 사용자 에이전트를 권한 부여 Endpoint으로 보내 흐름을 시작합니다. 클라이언트에는 클라이언트 식별자, 요청 된 범위, 로컬 상태 및 액세스 권한이 부여 (또는 거부)되면 권한 부여 서버가 사용자 에이전트를 다시 보낼 리디렉션 URI가 포함됩니다.

(B) 권한 부여 서버는 사용자 에이전트를 통해 리소스 소유자를 인증하고 리소스 소유자가 클라이언트의 액세스 요청을 허용 또는 거부할지 여부를 설정합니다.

(C) 리소스 소유자가 액세스 권한을 부여한다고 가정하면 권한 부여 서버는 이전에 제공된 리디렉션 URI를 사용하여 사용자 에이전트를 다시 클라이언트로 리디렉션합니다 (요청시 또는 클라이언트 등록 중에). 리디렉션 URI에는 인증 코드와 이전에 클라이언트가 제공하는 모든 로컬 상태가 포함됩니다.

(D) 클라이언트는 이전 단계에서받은 인증 코드를 포함하여 인증 서버의 토큰 Endpoint에서 액세스 토큰을 요청합니다. 요청을 할 때 클라이언트는 권한 부여 서버로 인증합니다. 클라이언트에는 확인을 위한 인증 코드를 얻는 데 사용되는 리디렉션 URI가 포함됩니다.

(E) 인증 서버는 클라이언트를 인증하고 인증 코드의 유효성을 검사하며 수신 된 리디렉션 URI가 단계 (C)에서 클라이언트를 리디렉션하는 데 사용 된 URI와 일치하는지 확인합니다. 유효한 경우 권한 부여 서버는 액세스 토큰 및 선택적으로 새로 고침 토큰으로 응답합니다.

#### 4.1.1. 권한 부여 요청

클라이언트는 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라 "application/x-www-form-urlencoded"형식을 사용하여 권한 부여 Endpoint URI의 쿼리 구성 요소에 다음 매개 변수를 추가하여 요청 URI를 구성합니다.

response_type

REQUIRED. 값은 "code"로 설정되어야합니다.

client_id

REQUIRED. [Section 2.2](#22-Client-Identifier)에 설명 된 클라이언트 식별자.

redirect_uri

OPTIONAL. [Section 3.1.2](#312-Redirection-Endpoint)에 설명 된대로.

scope

OPTIONAL. [Section 3.3](#33-Access-Token-Scope)에 설명 된 액세스 요청의 범위.

state

RECOMMENDED. 요청과 콜백 사이의 상태를 유지하기 위해 클라이언트가 사용하는 불투명 한 값입니다. 권한 부여 서버는 사용자 에이전트를 클라이언트로 다시 리디렉션 할 때이 값을 포함합니다. 매개 변수는 [Section 10.12](#102-Client-Impersonation)에 설명 된대로 교차 사이트 요청 위조를 방지하기 위해 사용되어야합니다 (SHOULD).

클라이언트는 HTTP 리디렉션 응답을 사용하거나 사용자 에이전트를 통해 사용할 수 있는 다른 방법을 사용하여 리소스 소유자를 구성된 URI로 보냅니다.

예를 들어 클라이언트는 TLS를 사용하여 다음 HTTP 요청을 수행하도록 사용자 에이전트에 지시합니다. (표시 목적으로 만 추가 줄 바꿈 포함):

    GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com

권한 부여 서버는 모든 필수 매개 변수가 있고 유효한지 확인하기 위해 요청의 유효성을 검증합니다. 요청이 유효한 경우 권한 부여 서버는 리소스 소유자를 인증하고 권한 결정을 얻습니다 (리소스 소유자에게 요청하거나 다른 방법을 통해 권한 부여를 설정).

결정이 설정되면 권한 부여 서버는 HTTP 리디렉션 응답을 사용하거나 사용자 에이전트를 통해 사용할 수있는 다른 수단을 사용하여 제공된 클라이언트 리디렉션 URI로 사용자 에이전트를 보냅니다.

#### 4.1.2. 권한 부여 응답

리소스 소유자가 액세스 요청을 허용하면 권한 부여 서버는 "application/x-www-form-urlencoded"형식을 사용하여 리디렉션 URI의 쿼리 구성 요소에 다음 매개 변수를 추가하여 권한 부여 코드를 발급하고 클라이언트에 전달합니다. 부록 B에 따라 :

code

REQUIRED. 권한 서버에서 생성 된 권한 코드입니다. 인증 코드는 누출 위험을 줄이기 위해 발급 된 직후 만료되어야합니다. 최대 인증 코드 수명은 10분입니다. 클라이언트는 인증 코드를 사용해서는 안됩니다.

     인증 코드가 두 번 이상 사용되는 경우 인증 서버는 요청을 거부해야하며 해당 인증 코드를 기반으로 이전에 발행 된 모든 토큰을 취소해야합니다 (가능한 경우). 인증 코드는 클라이언트 식별자 및 리디렉션 URI에 바인딩됩니다.

state

REQUIRED. 클라이언트 권한 요청에 "state"매개 변수가있는 경우 필수입니다. 클라이언트로부터받은 정확한 값입니다.

예를 들어 권한 부여 서버는 다음 HTTP 응답을 전송하여 사용자 에이전트를 리디렉션합니다.:

     HTTP/1.1 302 Found
     Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA
               &state=xyz

클라이언트는 인식되지 않는 응답 매개 변수를 무시해야합니다. 인증 코드 문자열 크기는 이 사양에 정의되지 않은 상태로 남아 있습니다. 클라이언트는 코드 값 크기에 대한 추측을 하지 않아야 합니다. 권한 부여 서버는 발행하는 값의 크기를 문서화해야합니다.

##### 4.1.2.1. 오류 응답

요청이 누락, 유효하지 않거나 일치하지 않는 리디렉션 URI로 인해 실패하거나 클라이언트 식별자가 누락되거나 유효하지 않은 경우 권한 부여 서버는 리소스 소유자에게 오류를 알려야하며 사용자 에이전트를 잘못된 리디렉션 URI로 자동 리디렉션해서는 안됩니다.

리소스 소유자가 액세스 요청을 거부하거나 누락되거나 잘못된 리디렉션 URI 이 외의 이유로 요청이 실패하는 경우 권한 부여 서버는 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)의 "application/x-www-form-urlencoded"형식을 사용하여 리디렉션 URI의 쿼리 구성 요소에 다음 매개 변수를 추가하여 클라이언트에 알립니다.:

error

REQUIRED. A single ASCII [[USASCII](https://tools.ietf.org/html/rfc6749#ref-USASCII)] error code from the
following:

         invalid_request
               요청에 필수 매개 변수가 누락되었거나, 잘못된 매개 변수 값이 포함되어 있거나,
               매개 변수가 두 번 이상 포함되어 있거나, 형식이 잘못되었습니다.

         unauthorized_client
               클라이언트는이 방법을 사용하여 인증 코드를 요청할 권한이 없습니다.

         access_denied
               자원 소유자 또는 권한 부여 서버가 요청을 거부했습니다.

         unsupported_response_type
               권한 서버는이 방법을 사용한 권한 코드 획득을 지원하지 않습니다.

         invalid_scope
               요청한 범위가 잘못되었거나 알 수 없거나 형식이 잘못되었습니다.

         server_error
               권한 부여 서버가 요청을 이행하지 못하게 하는 예기치 않은 조건을 발견했습니다.
               (이 오류 코드는 500 내부 서버 오류 HTTP 상태 코드를 HTTP 리디렉션을 통해
               클라이언트로 반환 할 수 없기 때문에 필요합니다.)

         temporarily_unavailable
               권한 부여 서버는 현재 서버의 일시적인 과부하 또는 유지 보수로 인해
               요청을 처리 할 수 ​​없습니다. (이 오류 코드는 503 Service Unavailable
               HTTP 상태 코드를 HTTP 리디렉션을 통해 클라이언트에 반환 할 수 없기 때문에
               필요합니다.)

          "오류"매개 변수의 값은
          %x20-21 / %x23-5B / %x5D-7E 세트 외부의 문자를 포함하면 안됩니다.

error_description

OPTIONAL. 클라이언트 개발자가 발생한 오류를 이해하는 데 사용되는 추가 정보를 제공하는 사람이 읽을 수있는 ASCII [[USASCII](https://tools.ietf.org/html/rfc6749#ref-USASCII)] 텍스트입니다. "error_description"매개 변수의 값은 %x20-21 / %x23-5B / %x5D-7E 세트 외부의 문자를 포함하면 안됩니다 (MUST NOT).

error_uri

OPTIONAL. 오류에 대한 정보가있는 사람이 읽을 수있는 웹 페이지를 식별하는 URI로, 클라이언트 개발자에게 오류에 대한 추가 정보를 제공하는 데 사용됩니다. "error_uri"매개 변수의 값은 URI 참조 구문을 준수해야하며 따라서 %x21 / %x23-5B / %x5D-7E 세트 외부의 문자를 포함하면 안됩니다.

state

REQUIRED. 클라이언트 인증 요청에 "state"매개 변수가있는 경우. 클라이언트로부터받은 정확한 값입니다.

예를 들어 권한 부여 서버는 다음 HTTP 응답을 전송하여 사용자 에이전트를 리디렉션합니다.:

     HTTP/1.1 302 Found
     Location: https://client.example.com/cb?error=access_denied&state=xyz

#### 4.1.3. 액세스 토큰 요청

클라이언트는 HTTP 요청 엔티티 본문에서 UTF-8의 문자 인코딩과 함께 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라 "application/x-www-form-urlencoded"형식을 사용하여 다음 매개 변수를 전송하여 토큰 엔드 포인트에 요청합니다.

grant_type

REQUIRED. 값은 "authorization_code"로 설정되어야합니다.

code

REQUIRED. 권하 부여 서버에서 받은 권한 부여 코드입니다.

redirect_uri

REQUIRED, "redirect_uri"매개 변수가 [Section 4.1.1](#411-Authorization-Request)에 설명 된대로 권한 부여 요청에 포함되었으며 해당 값이 동일해야합니다.

client_id

REQUIRED. 클라이언트가 [Section 3.2.1](#321-Client-Authentication)에 설명 된대로 권한 부여 서버로 인증하지 않는 경우 필수입니다.

클라이언트 유형이 기밀이거나 클라이언트가 클라이언트 자격 증명을 발급받은 경우 (또는 다른 인증 요구 사항이 할당 된 경우) 클라이언트는 [Section 3.2.1](#321-Client-Authentication)에 설명 된대로 인증 서버로 인증해야합니다.

예를 들어 클라이언트는 TLS를 사용하여 다음 HTTP 요청을 수행합니다. (표시 목적으로 만 추가 줄 바꿈 포함):

     POST /token HTTP/1.1
     Host: server.example.com
     Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
     Content-Type: application/x-www-form-urlencoded

     grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA
     &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb

권한 부여 서버는 다음을 수행해야합니다.

o 기밀 클라이언트 또는 클라이언트 자격 증명 (또는 기타 인증 요구 사항)이 발급 된 클라이언트에 대해 클라이언트 인증을 요구합니다.

o 클라이언트 인증이 포함 된 경우 클라이언트 인증

o 인증 코드가 인증 된 기밀 클라이언트에 발행되었는지 확인하거나 클라이언트가 공용 인 경우 코드가 요청에서 "client_id"에 발행되었는지 확인하십시오.

o 인증 코드가 유효한지 확인하고

o [Section 4.1.1](#411-Authorization-Request)에 설명 된대로 "redirect_uri"매개 변수가 초기 권한 요청에 포함 된 경우 "redirect_uri"매개 변수가 있는지 확인하고 포함 된 경우 해당 값이 동일한 지 확인하십시오.

#### 4.1.4. 액세스 토큰 응답

액세스 토큰 요청이 유효하고 권한 부여 된 경우 권한 부여 서버는 [Section 5.1](#51-Successful-Response)에 설명 된대로 액세스 토큰과 선택적으로 새로 고침 토큰을 발급합니다. 요청 클라이언트 인증이 실패하거나 유효하지 않은 경우 권한 부여 서버는 [Section 5.2](#52-Error-Response)에 설명 된대로 오류 응답을 반환합니다.

성공적인 응답의 예 :

     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
       "example_parameter":"example_value"
     }

### 4.2. 암시 적 부여

암시 적 권한 부여 유형은 액세스 토큰을 얻는 데 사용되며 (새로 고침 토큰 발급을 지원하지 않음) 특정 리디렉션 URI를 작동하는 것으로 알려진 공용 클라이언트에 최적화됩니다. 이러한 클라이언트는 일반적으로 JavaScript와 같은 스크립팅 언어를 사용하는 브라우저에서 구현됩니다.

이것은 리디렉션 기반 흐름이므로 클라이언트는 리소스 소유자의 사용자 에이전트 (일반적으로 웹 브라우저)와 상호 작용할 수 있어야하며 권한 부여 서버에서 들어오는 요청 (리디렉션을 통해)을 받을 수 있어야합니다.

클라이언트가 별도의 인증 요청과 액세스 토큰을 요청하는 인증 코드 부여 유형과 달리 클라이언트는 인증 요청의 결과로 액세스 토큰을받습니다.

암시 적 부여 유형에는 클라이언트 인증이 포함되지 않으며 리소스 소유자의 존재와 리디렉션 URI의 등록에 의존합니다. 액세스 토큰은 리디렉션 URI로 인코딩되기 때문에 리소스 소유자 및 동일한 장치에있는 다른 응용 프로그램에 노출 될 수 있습니다.

     +----------+
     | Resource |
     |  Owner   |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier     +---------------+
     |         -+----(A)-- & Redirection URI --->|               |
     |  User-   |                                | Authorization |
     |  Agent  -|----(B)-- User authenticates -->|     Server    |
     |          |                                |               |
     |          |<---(C)--- Redirection URI ----<|               |
     |          |          with Access Token     +---------------+
     |          |            in Fragment
     |          |                                +---------------+
     |          |----(D)--- Redirection URI ---->|   Web-Hosted  |
     |          |          without Fragment      |     Client    |
     |          |                                |    Resource   |
     |     (F)  |<---(E)------- Script ---------<|               |
     |          |                                +---------------+
     +-|--------+
       |    |
      (A)  (G) Access Token
       |    |
       ^    v
     +---------+
     |         |
     |  Client |
     |         |
     +---------+

      Note: The lines illustrating steps (A) and (B) are broken into two
      parts as they pass through the user-agent.

                       Figure 4: Implicit Grant Flow

그림 4에 표시된 흐름에는 다음 단계가 포함됩니다.:

(A) 클라이언트는 리소스 소유자의 사용자 에이전트를 권한 부여 Endpoint으로 보내 흐름을 시작합니다. 클라이언트에는 클라이언트 식별자, 요청 된 범위, 로컬 상태 및 액세스 권한이 부여 (또는 거부)되면 권한 부여 서버가 사용자 에이전트를 다시 보낼 리디렉션 URI가 포함됩니다.

(B) 권한 부여 서버는 사용자 에이전트를 통해 리소스 소유자를 인증하고 리소스 소유자가 클라이언트의 액세스 요청을 허용 또는 거부할지 여부를 설정합니다.

(C) 리소스 소유자가 액세스 권한을 부여한다고 가정하면 권한 부여 서버는 이전에 제공된 리디렉션 URI를 사용하여 사용자 에이전트를 클라이언트로 다시 리디렉션합니다. 리디렉션 URI는 URI에 액세스 토큰을 포함합니다.

(D) 사용자 에이전트는 웹 호스팅 클라이언트 리소스 ([[RFC2616](https://tools.ietf.org/html/rfc2616)])에 따른 fragment을 포함하지 않음)에 요청을하여 리디렉션 지침을 따릅니다. 사용자 에이전트는 fragment 정보를 로컬로 유지합니다.

(E) 웹 호스팅 클라이언트 리소스는 사용자 에이전트가 보유한 fragment을 포함하는 전체 리디렉션 URI에 액세스하고 액세스 토큰 (및 기타 매개 변수)을 추출 할 수있는 웹 페이지 (일반적으로 스크립트가 포함 된 HTML 문서)를 반환합니다. fragment내 포함되어 있습니다.

(F) 사용자 에이전트는 웹 호스팅 클라이언트 리소스에서 제공하는 스크립트를 로컬로 실행하여 액세스 토큰을 추출합니다.

(G) 사용자 에이전트는 액세스 토큰을 클라이언트에 전달합니다.

암시 적 허용 사용에 대한 배경 정보는 [Sections 1.3.2](#132-Implicit) and [9](#9-Native-Applications)를 참조하십시오.

암시 적 허용을 사용할 때 중요한 보안 고려 사항은 [Sections 10.3](#103-Access-Tokens) and [10.16](#1016-Misuse-of-Access-Token-to-Impersonate-Resource-Owner-in-Implicit-Flow)을 참조하십시오.

#### 4.2.1. 권한 부여 요청

클라이언트는 [Appendix B](<(#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)>)에 따라 "application/x-www-form-urlencoded"형식을 사용하여 권한 부여 Endpoint URI의 쿼리 구성 요소에 다음 매개 변수를 추가하여 요청 URI를 구성합니다.

response_type

REQUIRED. 값은 "token"으로 설정되어야합니다.

client_id

REQUIRED. [Section 2.2](#22-Client-Identifier)에 설명 된 클라이언트 식별자.

redirect_uri

OPTIONAL. [Section 3.1.2](#312-Redirection-Endpoint)에 설명 된대로.

scope

OPTIONAL. [Section 3.3](#33-Access-Token-Scope)에 설명 된 액세스 요청의 범위.

state

RECOMMENDED. 요청과 콜백 사이의 상태를 유지하기 위해 클라이언트가 사용하는 불투명 한 값입니다. 권한 부여 서버는 사용자 에이전트를 클라이언트로 다시 리디렉션 할 때 이 값을 포함합니다. 매개 변수는 [Section 10.12](#1012-Cross-Site-Request-Forgery)에 설명 된대로 교차 사이트 요청 위조를 방지하기 위해 사용되어야합니다 (SHOULD).

클라이언트는 HTTP 리디렉션 응답을 사용하거나 사용자 에이전트를 통해 사용할 수있는 다른 방법을 사용하여 리소스 소유자를 구성된 URI로 보냅니다.

예를 들어 클라이언트는 TLS를 사용하여 다음 HTTP 요청을 수행하도록 사용자 에이전트에 지시합니다. (표시 목적으로 만 추가 줄 바꿈 포함):

    GET /authorize?response_type=token&client_id=s6BhdRkqt3&state=xyz
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com

권한 부여 서버는 모든 필수 매개 변수가 있고 유효한지 확인하기 위해 요청의 유효성을 검증합니다. 권한 서버는 액세스 토큰을 리디렉션 할 리디렉션 URI가 [Section 3.1.2](#312-Redirection-Endpoint)에 설명 된대로 클라이언트가 등록한 리디렉션 URI와 일치하는지 확인해야합니다.

요청이 유효한 경우 권한 부여 서버는 리소스 소유자를 인증하고 권한 결정을 얻습니다 (리소스 소유자에게 요청하거나 다른 방법을 통해 승인을 설정).

결정이 설정되면 권한 부여 서버는 HTTP 리디렉션 응답을 사용하거나 사용자 에이전트를 통해 사용할 수있는 다른 수단을 사용하여 제공된 클라이언트 리디렉션 URI로 사용자 에이전트를 보냅니다.

#### 4.2.2. 액세스 토큰 응답

리소스 소유자가 액세스 요청을 허용하면 권한 부여 서버는 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)의 "application/x-www-form-urlencoded"형식을 사용하여 리디렉션 URI의 fragment 구성 요소에 다음 매개 변수를 추가하여 액세스 토큰을 발급하고 클라이언트에 전달합니다.:

access_token

REQUIRED. 권한 부여 서버에서 발급 한 액세스 토큰입니다.

token_type

REQUIRED. [Section 7.1](#71-Access-Token-Types)에 설명 된대로 발행 된 토큰의 유형. 값은 대소 문자를 구분하지 않습니다.

expires_in

RECOMMENDED. 액세스 토큰의 수명(초)입니다. 예를 들어, "3600"값은 응답이 생성 된 후 1 시간 후에 액세스 토큰이 만료됨을 나타냅니다. 생략 된 경우 권한 부여 서버는 다른 수단을 통해 만료 시간을 제공하거나 기본값을 문서화해야합니다.

scope

OPTIONAL, 클라이언트가 요청한 scope과 동일한 경우; 그렇지 않으면 REQUIRED. [Section 3.3](#33-Access-Token-Scope)에 설명 된 액세스 토큰의 범위.

state

클라이언트 권한 요청에 "state"매개 변수가있는 경우 REQUIRED. 클라이언트로부터 받은 정확한 값입니다.

권한 부여 서버는 새로 고침 토큰을 발행해서는 안됩니다.

예를 들어 권한 부여 서버는 다음 HTTP 응답을 전송하여 사용자 에이전트를 리디렉션합니다. (표시 용으로 만 추가 줄 바꿈 포함):

     HTTP/1.1 302 Found
     Location: http://example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA
               &state=xyz&token_type=example&expires_in=3600

개발자는 일부 사용자 에이전트가 HTTP "Location"응답 헤더 필드에 조각 구성 요소를 포함하는 것을 지원하지 않는다는 점에 유의해야합니다. 이러한 클라이언트는 3xx 리디렉션 응답 이외의 다른 방법을 사용하여 클라이언트를 리디렉션해야합니다. 예를 들어 리디렉션 URI에 연결된 작업이있는 'continue'버튼이 포함 된 HTML 페이지를 반환합니다.

클라이언트는 인식되지 않는 응답 매개 변수를 무시해야합니다. 액세스 토큰 문자열 크기는 이 사양에 정의되지 않은 상태로 남아 있습니다. 클라이언트는 값 크기에 대한 추측을 피해야합니다. 권한 부여 서버는 발행하는 값의 크기를 문서화해야합니다.

##### 4.2.2.1. 오류 응답

요청이 누락, 유효하지 않거나 일치하지 않는 리디렉션 URI로 인해 실패하거나 클라이언트 식별자가 누락되거나 유효하지 않은 경우 권한 부여 서버는 리소스 소유자에게 오류를 알려야하며 사용자 에이전트를 잘못된 리디렉션 URI로 자동 리디렉션해서는 안됩니다.

리소스 소유자가 액세스 요청을 거부하거나 누락되거나 잘못된 리디렉션 URI 이외의 이유로 요청이 실패하는 경우 권한 부여 서버는 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)의 "application/x-www-form-urlencoded"형식을 사용하여 리디렉션 URI의 fragment 구성 요소에 다음 매개 변수를 추가하여 클라이언트에 알립니다.:

error

REQUIRED. A single ASCII [[USASCII](https://tools.ietf.org/html/rfc6749#ref-USASCII)] error code from the following:

         invalid_request
               요청에 필수 매개 변수가 누락되었거나, 잘못된 매개 변수 값이 포함되어 있거나,
               매개 변수가 두 번 이상 포함되어 있거나, 형식이 잘못되었습니다.

         unauthorized_client
               클라이언트는 이 방법을 사용하여 액세스 토큰을 요청할 권한이 없습니다.

         access_denied
               자원 소유자 또는 권한 부여 서버가 요청을 거부했습니다.

         unsupported_response_type
               권한 부여 서버는 이 방법을 사용한 액세스 토큰 획득을 지원하지 않습니다.

         invalid_scope
               요청한 범위가 잘못되었거나 알 수 없거나 형식이 잘못되었습니다.

         server_error
               권한 부여 서버가 요청을 이행하지 못하게하는 예기치 않은 조건을 발견했습니다.
               (이 오류 코드는 500 내부 서버 오류 HTTP 상태 코드를 HTTP 리디렉션을 통해
               클라이언트로 반환 할 수 없기 때문에 필요합니다.)

         temporarily_unavailable
               권한 부여 서버는 현재 서버의 일시적인 과부하 또는 유지 보수로 인해
               요청을 처리 할 수 ​​없습니다.
               (이 오류 코드는 503 Service Unavailable HTTP 상태 코드를
               HTTP 리디렉션을 통해 클라이언트에 반환 할 수 없기 때문에 필요합니다.)

         "오류"매개 변수의 값은 %x20-21 / %x23-5B / %x5D-7E 세트 외부의 문자를
         포함하면 안됩니다.

error_description

OPTIONAL. 클라이언트 개발자가 발생한 오류를 이해하는 데 사용되는 추가 정보를 제공하는 사람이 읽을 수있는 ASCII [[USASCII](https://tools.ietf.org/html/rfc6749#ref-USASCII)] 텍스트입니다. "error_description"매개 변수의 값은 %x20-21 / %x23-5B / %x5D-7E 세트 외부의 문자를 포함하면 안됩니다 (MUST NOT).

error_uri

OPTIONAL. 오류에 대한 정보가있는 사람이 읽을 수 있는 웹 페이지를 식별하는 URI로, 클라이언트 개발자에게 오류에 대한 추가 정보를 제공하는 데 사용됩니다. "error_uri"매개 변수의 값은 URI 참조 구문을 준수해야하며 따라서 %x21 / %x23-5B / %x5D-7E 세트 외부의 문자를 포함하면 안됩니다.

state

클라이언트 인증 요청에 "state"매개 변수가있는 경우 REQUIRED. 클라이언트로부터받은 정확한 값입니다.

예를 들어 권한 부여 서버는 다음 HTTP 응답을 전송하여 사용자 에이전트를 리디렉션합니다.

     HTTP/1.1 302 Found
     Location: https://client.example.com/cb#error=access_denied&state=xyz

### 4.3. 리소스 소유자 암호 자격 증명 부여

리소스 소유자 암호 자격 증명 부여 유형은 리소스 소유자가 장치 운영 체제 또는 높은 권한을 가진 응용 프로그램과 같이 클라이언트와 신뢰 관계가있는 경우에 적합합니다. 권한 부여 서버는 이 권한 부여 유형을 활성화 할 때 특별한 주의를 기울여야하며 다른 흐름이 실행 가능하지 않은 경우에만 허용해야합니다.

이 부여 유형은 리소스 소유자의 자격 증명 (일반적으로 대화 형 양식을 사용하는 사용자 이름 및 암호)을 얻을 수있는 클라이언트에 적합합니다. 또한 저장된 자격 증명을 액세스 토큰으로 변환하여 HTTP 기본 또는 다이제스트 인증과 같은 직접 인증 체계를 사용하는 기존 클라이언트를 OAuth로 마이그레이션하는 데 사용됩니다.

     +----------+
     | Resource |
     |  Owner   |
     |          |
     +----------+
          v
          |    Resource Owner
         (A) Password Credentials
          |
          v
     +---------+                                  +---------------+
     |         |>--(B)---- Resource Owner ------->|               |
     |         |         Password Credentials     | Authorization |
     | Client  |                                  |     Server    |
     |         |<--(C)---- Access Token ---------<|               |
     |         |    (w/ Optional Refresh Token)   |               |
     +---------+                                  +---------------+

            Figure 5: Resource Owner Password Credentials Flow

그림 5에 표시된 흐름에는 다음 단계가 포함됩니다.

(A) 리소스 소유자는 클라이언트에게 사용자 이름과 암호를 제공합니다.

(B) 클라이언트는 리소스 소유자로부터받은 자격 증명을 포함하여 권한 부여 서버의 토큰 Endpoint에서 액세스 토큰을 요청합니다. 요청을 할 때 클라이언트는 권한 부여 서버로 인증합니다.

(C) 권한 부여 서버는 클라이언트를 인증하고 리소스 소유자 자격 증명의 유효성을 검사하고 유효한 경우 액세스 토큰을 발급합니다.

#### 4.3.1. 권한 부여 요청 및 응답

클라이언트가 리소스 소유자 자격 증명을 얻는 방법은 이 사양의 범위를 벗어납니다. 클라이언트는 액세스 토큰이 확보되면 자격 증명을 폐기해야합니다.

#### 4.3.2. 액세스 토큰 요청

클라이언트는 HTTP 요청 엔티티 본문에 UTF-8 문자 인코딩과 함께 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라 "application/x-www-form-urlencoded"형식을 사용하여 다음 매개 변수를 추가하여 토큰 엔드 포인트에 요청합니다.

grant_type

REQUIRED. 값은 "password"로 설정해야합니다.

username

REQUIRED. 리소스 소유자 사용자 이름입니다.

password

REQUIRED. 자원 소유자 비밀번호입니다.

scope

OPTIONAL. [Section 3.3](#33-Access-Token-Scope)에 설명 된 액세스 요청의 범위.

클라이언트 유형이 기밀이거나 클라이언트가 클라이언트 자격 증명을 발급받은 경우 (또는 다른 인증 요구 사항이 할당 된 경우) 클라이언트는 [Section 3.2.1](#321-Client-Authentication)에 설명 된대로 인증 서버로 인증해야합니다.

예를 들어 클라이언트는 전송 계층 보안을 사용하여 다음 HTTP 요청을 수행합니다. (표시 목적으로 만 추가 줄 바꿈 포함):

     POST /token HTTP/1.1
     Host: server.example.com
     Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
     Content-Type: application/x-www-form-urlencoded

     grant_type=password&username=johndoe&password=A3ddj3w

권한 부여 서버는 다음을 수행해야합니다:

o 기밀 클라이언트 또는 클라이언트 자격 증명 (또는 기타 인증 요구 사항)이 발급 된 클라이언트에 대해 클라이언트 인증을 요구합니다.

o 클라이언트 인증이 포함 된 경우 클라이언트를 인증합니다.

o 기존 비밀번호 검증 알고리즘을 사용하여 자원 소유자 비밀번호 정보를 검증합니다.

이 액세스 토큰 요청은 리소스 소유자의 비밀번호를 사용하므로 권한 부여 서버는 무차별 대입 공격 (예 : 속도 제한 사용 또는 경고 생성)으로부터 엔드 포인트를 보호해야합니다.

#### 4.3.3. 액세스 토큰 응답

액세스 토큰 요청이 유효하고 권한 부여 된 경우 권한 부여 서버는 [Section 5.1](#51-Successful-Response)에 설명 된대로 액세스 토큰과 선택적으로 새로 고침 토큰을 발급합니다. 요청이 클라이언트 인증에 실패했거나 유효하지 않은 경우 권한 부여 서버는 [Section 5.2](#52-Error-Response)에 설명 된대로 오류 응답을 반환합니다.

성공적인 응답의 예 :

     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
       "example_parameter":"example_value"
     }

### 4.4. 클라이언트 자격 증명 부여

클라이언트는 클라이언트가 제어하에있는 보호 된 리소스에 대한 액세스를 요청할 때 또는 이전에 권한 부여 서버에 배치 된 다른 리소스 소유자(이 방법은 이 사양의 범위를 벗어납니다)의 액세스를 요청할 때 클라이언트 자격 증명 (또는 기타 지원되는 인증 수단) 만 사용하여 액세스 토큰을 요청할 수 있습니다.

클라이언트 자격 증명 부여 유형은 기밀 클라이언트 만 사용해야합니다.

### 4.4. Client Credentials Grant

     +---------+                                  +---------------+
     |         |                                  |               |
     |         |>--(A)- Client Authentication --->| Authorization |
     | Client  |                                  |     Server    |
     |         |<--(B)---- Access Token ---------<|               |
     |         |                                  |               |
     +---------+                                  +---------------+

                     Figure 6: Client Credentials Flow

그림 6에 표시된 흐름에는 다음 단계가 포함됩니다.

(A) 클라이언트는 권한 부여 서버로 인증하고 토큰 Endpoint에서 액세스 토큰을 요청합니다.

(B) 인증 서버는 클라이언트를 인증하고 유효한 경우 액세스 토큰을 발급합니다.

#### 4.4.1. 권한 부여 요청 및 응답

클라이언트 인증이 권한 부여로 사용되므로 추가 권한 요청이 필요하지 않습니다.

#### 4.4.2. 액세스 토큰 요청

클라이언트는 HTTP 요청 엔티티 본문에 UTF-8 문자 인코딩과 함께 [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type)에 따라 "application/x-www-form-urlencoded"형식을 사용하여 다음 매개 변수를 추가하여 토큰 엔드 포인트에 요청을합니다.

grant_type

REQUIRED. 값은 "client_credentials"로 설정되어야합니다.

scope

OPTIONAL. [Section 3.3](#33-Access-Token-Scope) 설명 된 액세스 요청의 범위.

클라이언트는 [Section 3.2.1](#321-Client-Authentication)에 설명 된대로 권한 부여 서버로 인증해야합니다.

예를 들어 클라이언트는 전송 계층 보안을 사용하여 다음 HTTP 요청을 수행합니다. (표시 목적으로 만 추가 줄 바꿈 포함):

     POST /token HTTP/1.1
     Host: server.example.com
     Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
     Content-Type: application/x-www-form-urlencoded

     grant_type=client_credentials

권한 서버는 클라이언트를 인증해야합니다.

#### 4.4.3. 액세스 토큰 응답

액세스 토큰 요청이 유효하고 권한이있는 경우 권한 부여 서버는 [Section 5.1](#51-Successful-Response)에 설명 된대로 액세스 토큰을 발급합니다. 새로 고침 토큰은 포함하지 않아야합니다. 요청이 클라이언트 인증에 실패했거나 유효하지 않은 경우 권한 부여 서버는 [Section 5.2](#52-Error-Response)에 설명 된대로 오류 응답을 반환합니다.

성공적인 응답의 예 :

     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "example_parameter":"example_value"
     }

클라이언트는 토큰 엔드 포인트의 "grant_type"매개 변수 값으로 절대 URI (권한 부여 서버에 의해 정의 됨)를 사용하여 부여 유형을 지정하고 필요한 추가 매개 변수를 추가하여 확장 부여 유형을 사용합니다.

예를 들어 [[OAuth-SAML2](https://tools.ietf.org/html/rfc6749#ref-OAuth-SAML2)]에서 정의한 SAML (Security Assertion Markup Language) 2.0 assertion 부여 유형을 사용하여 액세스 토큰을 요청하려면 클라이언트가 TLS를 사용하여 다음 HTTP 요청을 수행 할 수 있습니다. (표시 목적으로 만 추가 줄 바꿈 포함):

### 4.5. Extension Grants

클라이언트는 토큰 엔드 포인트의 "grant_type"매개 변수 값으로 절대 URI (권한 부여 서버에 의해 정의 됨)를 사용하여 부여 유형을 지정하고 필요한 추가 매개 변수를 추가하여 확장 부여 유형을 사용합니다.

예를 들어 [[OAuth-SAML2](https://tools.ietf.org/html/rfc6749#ref-OAuth-SAML2)]에서 정의한 SAML (Security Assertion Markup Language) 2.0 assertion 부여 유형을 사용하여 액세스 토큰을 요청하려면 클라이언트가 TLS를 사용하여 다음 HTTP 요청을 수행 할 수 있습니다. (표시 목적으로 만 추가 줄 바꿈 포함):

     POST /token HTTP/1.1
     Host: server.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Asaml2-
     bearer&assertion=PEFzc2VydGlvbiBJc3N1ZUluc3RhbnQ9IjIwMTEtMDU
     [...omitted for brevity...]aG5TdGF0ZW1lbnQ-PC9Bc3NlcnRpb24-

액세스 토큰 요청이 유효하고 권한이있는 경우 권한 부여 서버는 [Section 5.1](#51-Successful-Response)에 설명 된대로 액세스 토큰과 선택적으로 새로 고침 토큰을 발급합니다. 요청이 클라이언트 인증에 실패했거나 유효하지 않은 경우 권한 부여 서버는 [Section 5.2](#52-Error-Response)에 설명 된대로 오류 응답을 반환합니다.

## 5. 액세스 토큰 발급

If the access token request is valid and authorized, the authorization server issues an access token and optional refresh token as described in [Section 5.1](#51-Successful-Response). If the request failed client authentication or is invalid, the authorization server returns an error response as described in [Section 5.2](#52-Error-Response).

### 5.1. Successful Response

The authorization server issues an access token and optional refresh
token, and constructs the response by adding the following parameters
to the entity-body of the HTTP response with a 200 (OK) status code:

access_token
REQUIRED. The access token issued by the authorization server.

token_type
REQUIRED. The type of the token issued as described in
[Section 7.1](#71-Access-Token-Types). Value is case insensitive.

expires_in
RECOMMENDED. The lifetime in seconds of the access token. For
example, the value "3600" denotes that the access token will
expire in one hour from the time the response was generated.
If omitted, the authorization server SHOULD provide the
expiration time via other means or document the default value.

refresh_token
OPTIONAL. The refresh token, which can be used to obtain new
access tokens using the same authorization grant as described
in [Section 6](#6-Refreshing-an-Access-Token).

scope
OPTIONAL, if identical to the scope requested by the client;
otherwise, REQUIRED. The scope of the access token as
described by [Section 3.3](#33-Access-Token-Scope).

The parameters are included in the entity-body of the HTTP response
using the "application/json" media type as defined by [[RFC4627](https://tools.ietf.org/html/rfc4627)]. The
parameters are serialized into a JavaScript Object Notation (JSON)
structure by adding each parameter at the highest structure level.
Parameter names and string values are included as JSON strings.
Numerical values are included as JSON numbers. The order of
parameters does not matter and can vary.

The authorization server MUST include the HTTP "Cache-Control"
response header field [[RFC2616](https://tools.ietf.org/html/rfc2616)] with a value of "no-store" in any
response containing tokens, credentials, or other sensitive
information, as well as the "Pragma" response header field [[RFC2616](https://tools.ietf.org/html/rfc2616)]
with a value of "no-cache".

For example:

     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
       "example_parameter":"example_value"
     }

The client MUST ignore unrecognized value names in the response. The
sizes of tokens and other values received from the authorization
server are left undefined. The client should avoid making
assumptions about value sizes. The authorization server SHOULD
document the size of any value it issues.

### 5.2. Error Response

The authorization server responds with an HTTP 400 (Bad Request)
status code (unless specified otherwise) and includes the following
parameters with the response:

error
REQUIRED. A single ASCII [[USASCII](https://tools.ietf.org/html/rfc6749#ref-USASCII)] error code from the
following:

         invalid_request
               The request is missing a required parameter, includes an
               unsupported parameter value (other than grant type),
               repeats a parameter, includes multiple credentials,
               utilizes more than one mechanism for authenticating the
               client, or is otherwise malformed.

         invalid_client
               Client authentication failed (e.g., unknown client, no
               client authentication included, or unsupported
               authentication method).  The authorization server MAY
               return an HTTP 401 (Unauthorized) status code to indicate
               which HTTP authentication schemes are supported.  If the
               client attempted to authenticate via the "Authorization"
               request header field, the authorization server MUST
               respond with an HTTP 401 (Unauthorized) status code and
               include the "WWW-Authenticate" response header field
               matching the authentication scheme used by the client.

         invalid_grant
               The provided authorization grant (e.g., authorization
               code, resource owner credentials) or refresh token is
               invalid, expired, revoked, does not match the redirection
               URI used in the authorization request, or was issued to
               another client.

         unauthorized_client
               The authenticated client is not authorized to use this
               authorization grant type.

         unsupported_grant_type
               The authorization grant type is not supported by the
               authorization server.

         invalid_scope
               The requested scope is invalid, unknown, malformed, or
               exceeds the scope granted by the resource owner.

         Values for the "error" parameter MUST NOT include characters
         outside the set %x20-21 / %x23-5B / %x5D-7E.

error_description
OPTIONAL. Human-readable ASCII [[USASCII](https://tools.ietf.org/html/rfc6749#ref-USASCII)] text providing
additional information, used to assist the client developer in
understanding the error that occurred.
Values for the "error_description" parameter MUST NOT include
characters outside the set %x20-21 / %x23-5B / %x5D-7E.

error_uri
OPTIONAL. A URI identifying a human-readable web page with
information about the error, used to provide the client
developer with additional information about the error.
Values for the "error_uri" parameter MUST conform to the
URI-reference syntax and thus MUST NOT include characters
outside the set %x21 / %x23-5B / %x5D-7E.

The parameters are included in the entity-body of the HTTP response
using the "application/json" media type as defined by [[RFC4627](https://tools.ietf.org/html/rfc4627)]. The
parameters are serialized into a JSON structure by adding each
parameter at the highest structure level. Parameter names and string
values are included as JSON strings. Numerical values are included
as JSON numbers. The order of parameters does not matter and can
vary.

For example:

     HTTP/1.1 400 Bad Request
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "error":"invalid_request"
     }

## 6. Refreshing an Access Token

If the authorization server issued a refresh token to the client, the
client makes a refresh request to the token endpoint by adding the
following parameters using the "application/x-www-form-urlencoded"
format per [Appendix B](#Appendix-B.-Use-of-application%2Fx-www-form-urlencoded-Media-Type) with a character encoding of UTF-8 in the HTTP
request entity-body:

grant_type
REQUIRED. Value MUST be set to "refresh_token".

refresh_token
REQUIRED. The refresh token issued to the client.

scope
OPTIONAL. The scope of the access request as described by
[Section 3.3](#33-Access-Token-Scope). The requested scope MUST NOT include any scope
not originally granted by the resource owner, and if omitted is
treated as equal to the scope originally granted by the
resource owner.

Because refresh tokens are typically long-lasting credentials used to
request additional access tokens, the refresh token is bound to the
client to which it was issued. If the client type is confidential or
the client was issued client credentials (or assigned other
authentication requirements), the client MUST authenticate with the
authorization server as described in [Section 3.2.1](#321-Client-Authentication).

For example, the client makes the following HTTP request using
transport-layer security (with extra line breaks for display purposes
only):

     POST /token HTTP/1.1
     Host: server.example.com
     Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
     Content-Type: application/x-www-form-urlencoded

     grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA

The authorization server MUST:

o require client authentication for confidential clients or for any
client that was issued client credentials (or with other
authentication requirements),

o authenticate the client if client authentication is included and
ensure that the refresh token was issued to the authenticated
client, and

o validate the refresh token.

If valid and authorized, the authorization server issues an access
token as described in [Section 5.1](#51-Successful-Response). If the request failed
verification or is invalid, the authorization server returns an error
response as described in [Section 5.2](#52-Error-Response).

The authorization server MAY issue a new refresh token, in which case
the client MUST discard the old refresh token and replace it with the
new refresh token. The authorization server MAY revoke the old
refresh token after issuing a new refresh token to the client. If a
new refresh token is issued, the refresh token scope MUST be
identical to that of the refresh token included by the client in the
request.

## 7. Accessing Protected Resources

The client accesses protected resources by presenting the access
token to the resource server. The resource server MUST validate the
access token and ensure that it has not expired and that its scope
covers the requested resource. The methods used by the resource
server to validate the access token (as well as any error responses)
are beyond the scope of this specification but generally involve an
interaction or coordination between the resource server and the
authorization server.

The method in which the client utilizes the access token to
authenticate with the resource server depends on the type of access
token issued by the authorization server. Typically, it involves
using the HTTP "Authorization" request header field [[RFC2617](https://tools.ietf.org/html/rfc2617)] with an
authentication scheme defined by the specification of the access
token type used, such as [[RFC6750](https://tools.ietf.org/html/rfc6750)].

### 7.1. Access Token Types

The access token type provides the client with the information
required to successfully utilize the access token to make a protected
resource request (along with type-specific attributes). The client
MUST NOT use an access token if it does not understand the token
type.

For example, the "bearer" token type defined in [[RFC6750](https://tools.ietf.org/html/rfc6750)] is utilized
by simply including the access token string in the request:

     GET /resource/1 HTTP/1.1
     Host: example.com
     Authorization: Bearer mF_9.B5f-4.1JqM

while the "mac" token type defined in [[OAuth-HTTP-MAC](https://tools.ietf.org/html/rfc6749#ref-OAuth-HTTP-MAC)] is utilized by
issuing a Message Authentication Code (MAC) key together with the
access token that is used to sign certain components of the HTTP
requests:

     GET /resource/1 HTTP/1.1
     Host: example.com
     Authorization: MAC id="h480djs93hd8",
                        nonce="274312:dj83hs9s",
                        mac="kDZvddkndxvhGRXZhvuDjEWhGeE="

The above examples are provided for illustration purposes only.
Developers are advised to consult the [[RFC6750](https://tools.ietf.org/html/rfc6750)] and [[OAuth-HTTP-MAC](https://tools.ietf.org/html/rfc6749#ref-OAuth-HTTP-MAC)]
specifications before use.

Each access token type definition specifies the additional attributes
(if any) sent to the client together with the "access_token" response
parameter. It also defines the HTTP authentication method used to
include the access token when making a protected resource request.

### 7.2. Error Response

If a resource access request fails, the resource server SHOULD inform
the client of the error. While the specifics of such error responses
are beyond the scope of this specification, this document establishes
a common registry in [Section 11.4](#114-OAuth-Extensions-Error-Registry) for error values to be shared among
OAuth token authentication schemes.

New authentication schemes designed primarily for OAuth token
authentication SHOULD define a mechanism for providing an error
status code to the client, in which the error values allowed are
registered in the error registry established by this specification.

Such schemes MAY limit the set of valid error codes to a subset of
the registered values. If the error code is returned using a named
parameter, the parameter name SHOULD be "error".

Other schemes capable of being used for OAuth token authentication,
but not primarily designed for that purpose, MAY bind their error
values to the registry in the same manner.

New authentication schemes MAY choose to also specify the use of the
"error_description" and "error_uri" parameters to return error
information in a manner parallel to their usage in this
specification.

## 8. Extensibility

### 8.1. Defining Access Token Types

Access token types can be defined in one of two ways: registered in
the Access Token Types registry (following the procedures in
[Section 11.1](#111-OAuth-Access-Token-Types-Registry)), or by using a unique absolute URI as its name.

Types utilizing a URI name SHOULD be limited to vendor-specific
implementations that are not commonly applicable, and are specific to
the implementation details of the resource server where they are
used.

All other types MUST be registered. Type names MUST conform to the
type-name ABNF. If the type definition includes a new HTTP
authentication scheme, the type name SHOULD be identical to the HTTP
authentication scheme name (as defined by [[RFC2617](https://tools.ietf.org/html/rfc2617)]). The token type
"example" is reserved for use in examples.

     type-name  = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

### 8.2. Defining New Endpoint Parameters

New request or response parameters for use with the authorization
endpoint or the token endpoint are defined and registered in the
OAuth Parameters registry following the procedure in [Section 11.2](#112-OAuth-Parameters-Registry).

Parameter names MUST conform to the param-name ABNF, and parameter
values syntax MUST be well-defined (e.g., using ABNF, or a reference
to the syntax of an existing parameter).

     param-name  = 1*name-char
     name-char   = "-" / "." / "_" / DIGIT / ALPHA

Unregistered vendor-specific parameter extensions that are not
commonly applicable and that are specific to the implementation
details of the authorization server where they are used SHOULD
utilize a vendor-specific prefix that is not likely to conflict with
other registered values (e.g., begin with 'companyname\_').

### 8.3. Defining New Authorization Grant Types

New authorization grant types can be defined by assigning them a
unique absolute URI for use with the "grant_type" parameter. If the
extension grant type requires additional token endpoint parameters,
they MUST be registered in the OAuth Parameters registry as described
by [Section 11.2](#112-OAuth-Parameters-Registry).

### 8.4. Defining New Authorization Endpoint Response Types

New response types for use with the authorization endpoint are
defined and registered in the Authorization Endpoint Response Types
registry following the procedure in [Section 11.3](#11..OAuth-Authorization-Endpoint-Response-Types-Registry). Response type
names MUST conform to the response-type ABNF.

     response-type  = response-name *( SP response-name )
     response-name  = 1*response-char
     response-char  = "_" / DIGIT / ALPHA

If a response type contains one or more space characters (%x20), it
is compared as a space-delimited list of values in which the order of
values does not matter. Only one order of values can be registered,
which covers all other arrangements of the same set of values.

For example, the response type "token code" is left undefined by this
specification. However, an extension can define and register the
"token code" response type. Once registered, the same combination
cannot be registered as "code token", but both values can be used to
denote the same response type.

### 8.5. Defining Additional Error Codes

In cases where protocol extensions (i.e., access token types,
extension parameters, or extension grant types) require additional
error codes to be used with the authorization code grant error
response ([Section 4.1.2.1](#4.1.2.1.-Error-Response)), the implicit grant error response
([Section 4.2.2.1](#4.2.2.1.-Error-Response)), the token error response ([Section 5.2](#52-Error-Response)), or the
resource access error response ([Section 7.2](#72-Error-Response)), such error codes MAY be
defined.

Extension error codes MUST be registered (following the procedures in
[Section 11.4](#114-OAuth-Extensions-Error-Registry)) if the extension they are used in conjunction with is a
registered access token type, a registered endpoint parameter, or an
extension grant type. Error codes used with unregistered extensions
MAY be registered.

Error codes MUST conform to the error ABNF and SHOULD be prefixed by
an identifying name when possible. For example, an error identifying
an invalid value set to the extension parameter "example" SHOULD be
named "example_invalid".

     error      = 1*error-char
     error-char = %x20-21 / %x23-5B / %x5D-7E

## 9. Native Applications

Native applications are clients installed and executed on the device
used by the resource owner (i.e., desktop application, native mobile
application). Native applications require special consideration
related to security, platform capabilities, and overall end-user
experience.

The authorization endpoint requires interaction between the client
and the resource owner's user-agent. Native applications can invoke
an external user-agent or embed a user-agent within the application.
For example:

o External user-agent - the native application can capture the
response from the authorization server using a redirection URI
with a scheme registered with the operating system to invoke the
client as the handler, manual copy-and-paste of the credentials,
running a local web server, installing a user-agent extension, or
by providing a redirection URI identifying a server-hosted
resource under the client's control, which in turn makes the
response available to the native application.

o Embedded user-agent - the native application obtains the response
by directly communicating with the embedded user-agent by
monitoring state changes emitted during the resource load, or
accessing the user-agent's cookies storage.

When choosing between an external or embedded user-agent, developers
should consider the following:

o An external user-agent may improve completion rate, as the
resource owner may already have an active session with the
authorization server, removing the need to re-authenticate. It
provides a familiar end-user experience and functionality. The

      resource owner may also rely on user-agent features or extensions
      to assist with authentication (e.g., password manager, 2-factor
      device reader).

o An embedded user-agent may offer improved usability, as it removes
the need to switch context and open new windows.

o An embedded user-agent poses a security challenge because resource
owners are authenticating in an unidentified window without access
to the visual protections found in most external user-agents. An
embedded user-agent educates end-users to trust unidentified
requests for authentication (making phishing attacks easier to
execute).

When choosing between the implicit grant type and the authorization
code grant type, the following should be considered:

o Native applications that use the authorization code grant type
SHOULD do so without using client credentials, due to the native
application's inability to keep client credentials confidential.

o When using the implicit grant type flow, a refresh token is not
returned, which requires repeating the authorization process once
the access token expires.

## 10. Security Considerations

As a flexible and extensible framework, OAuth's security
considerations depend on many factors. The following sections
provide implementers with security guidelines focused on the three
client profiles described in [Section 2.1](#21-Client-Types): web application,
user-agent-based application, and native application.

A comprehensive OAuth security model and analysis, as well as
background for the protocol design, is provided by
[[OAuth-THREATMODEL](https://tools.ietf.org/html/rfc6749#ref-OAuth-THREATMODEL)].

### 10.1. Client Authentication

The authorization server establishes client credentials with web
application clients for the purpose of client authentication. The
authorization server is encouraged to consider stronger client
authentication means than a client password. Web application clients
MUST ensure confidentiality of client passwords and other client
credentials.

The authorization server MUST NOT issue client passwords or other
client credentials to native application or user-agent-based
application clients for the purpose of client authentication. The
authorization server MAY issue a client password or other credentials
for a specific installation of a native application client on a
specific device.

When client authentication is not possible, the authorization server
SHOULD employ other means to validate the client's identity -- for
example, by requiring the registration of the client redirection URI
or enlisting the resource owner to confirm identity. A valid
redirection URI is not sufficient to verify the client's identity
when asking for resource owner authorization but can be used to
prevent delivering credentials to a counterfeit client after
obtaining resource owner authorization.

The authorization server must consider the security implications of
interacting with unauthenticated clients and take measures to limit
the potential exposure of other credentials (e.g., refresh tokens)
issued to such clients.

### 10.2. Client Impersonation

A malicious client can impersonate another client and obtain access
to protected resources if the impersonated client fails to, or is
unable to, keep its client credentials confidential.

The authorization server MUST authenticate the client whenever
possible. If the authorization server cannot authenticate the client
due to the client's nature, the authorization server MUST require the
registration of any redirection URI used for receiving authorization
responses and SHOULD utilize other means to protect resource owners
from such potentially malicious clients. For example, the
authorization server can engage the resource owner to assist in
identifying the client and its origin.

The authorization server SHOULD enforce explicit resource owner
authentication and provide the resource owner with information about
the client and the requested authorization scope and lifetime. It is
up to the resource owner to review the information in the context of
the current client and to authorize or deny the request.

The authorization server SHOULD NOT process repeated authorization
requests automatically (without active resource owner interaction)
without authenticating the client or relying on other measures to
ensure that the repeated request comes from the original client and
not an impersonator.

### 10.3. Access Tokens

Access token credentials (as well as any confidential access token
attributes) MUST be kept confidential in transit and storage, and
only shared among the authorization server, the resource servers the
access token is valid for, and the client to whom the access token is
issued. Access token credentials MUST only be transmitted using TLS
as described in [Section 1.6](#16-TLS-Version) with server authentication as defined by
[[RFC2818](https://tools.ietf.org/html/rfc2818)].

When using the implicit grant type, the access token is transmitted
in the URI fragment, which can expose it to unauthorized parties.

The authorization server MUST ensure that access tokens cannot be
generated, modified, or guessed to produce valid access tokens by
unauthorized parties.

The client SHOULD request access tokens with the minimal scope
necessary. The authorization server SHOULD take the client identity
into account when choosing how to honor the requested scope and MAY
issue an access token with less rights than requested.

This specification does not provide any methods for the resource
server to ensure that an access token presented to it by a given
client was issued to that client by the authorization server.

### 10.4. Refresh Tokens

Authorization servers MAY issue refresh tokens to web application
clients and native application clients.

Refresh tokens MUST be kept confidential in transit and storage, and
shared only among the authorization server and the client to whom the
refresh tokens were issued. The authorization server MUST maintain
the binding between a refresh token and the client to whom it was
issued. Refresh tokens MUST only be transmitted using TLS as
described in [Section 1.6](#16-TLS-Version) with server authentication as defined by
[[RFC2818](https://tools.ietf.org/html/rfc2818)].

The authorization server MUST verify the binding between the refresh
token and client identity whenever the client identity can be
authenticated. When client authentication is not possible, the
authorization server SHOULD deploy other means to detect refresh
token abuse.

For example, the authorization server could employ refresh token
rotation in which a new refresh token is issued with every access
token refresh response. The previous refresh token is invalidated

but retained by the authorization server. If a refresh token is
compromised and subsequently used by both the attacker and the
legitimate client, one of them will present an invalidated refresh
token, which will inform the authorization server of the breach.

The authorization server MUST ensure that refresh tokens cannot be
generated, modified, or guessed to produce valid refresh tokens by
unauthorized parties.

### 10.5. Authorization Codes

The transmission of authorization codes SHOULD be made over a secure
channel, and the client SHOULD require the use of TLS with its
redirection URI if the URI identifies a network resource. Since
authorization codes are transmitted via user-agent redirections, they
could potentially be disclosed through user-agent history and HTTP
referrer headers.

Authorization codes operate as plaintext bearer credentials, used to
verify that the resource owner who granted authorization at the
authorization server is the same resource owner returning to the
client to complete the process. Therefore, if the client relies on
the authorization code for its own resource owner authentication, the
client redirection endpoint MUST require the use of TLS.

Authorization codes MUST be short lived and single-use. If the
authorization server observes multiple attempts to exchange an
authorization code for an access token, the authorization server
SHOULD attempt to revoke all access tokens already granted based on
the compromised authorization code.

If the client can be authenticated, the authorization servers MUST
authenticate the client and ensure that the authorization code was
issued to the same client.

### 10.6. Authorization Code Redirection URI Manipulation

When requesting authorization using the authorization code grant
type, the client can specify a redirection URI via the "redirect_uri"
parameter. If an attacker can manipulate the value of the
redirection URI, it can cause the authorization server to redirect
the resource owner user-agent to a URI under the control of the
attacker with the authorization code.

An attacker can create an account at a legitimate client and initiate
the authorization flow. When the attacker's user-agent is sent to
the authorization server to grant access, the attacker grabs the
authorization URI provided by the legitimate client and replaces the

client's redirection URI with a URI under the control of the
attacker. The attacker then tricks the victim into following the
manipulated link to authorize access to the legitimate client.

Once at the authorization server, the victim is prompted with a
normal, valid request on behalf of a legitimate and trusted client,
and authorizes the request. The victim is then redirected to an
endpoint under the control of the attacker with the authorization
code. The attacker completes the authorization flow by sending the
authorization code to the client using the original redirection URI
provided by the client. The client exchanges the authorization code
with an access token and links it to the attacker's client account,
which can now gain access to the protected resources authorized by
the victim (via the client).

In order to prevent such an attack, the authorization server MUST
ensure that the redirection URI used to obtain the authorization code
is identical to the redirection URI provided when exchanging the
authorization code for an access token. The authorization server
MUST require public clients and SHOULD require confidential clients
to register their redirection URIs. If a redirection URI is provided
in the request, the authorization server MUST validate it against the
registered value.

### 10.7. Resource Owner Password Credentials

The resource owner password credentials grant type is often used for
legacy or migration reasons. It reduces the overall risk of storing
usernames and passwords by the client but does not eliminate the need
to expose highly privileged credentials to the client.

This grant type carries a higher risk than other grant types because
it maintains the password anti-pattern this protocol seeks to avoid.
The client could abuse the password, or the password could
unintentionally be disclosed to an attacker (e.g., via log files or
other records kept by the client).

Additionally, because the resource owner does not have control over
the authorization process (the resource owner's involvement ends when
it hands over its credentials to the client), the client can obtain
access tokens with a broader scope than desired by the resource
owner. The authorization server should consider the scope and
lifetime of access tokens issued via this grant type.

The authorization server and client SHOULD minimize use of this grant
type and utilize other grant types whenever possible.

### 10.8. Request Confidentiality

Access tokens, refresh tokens, resource owner passwords, and client
credentials MUST NOT be transmitted in the clear. Authorization
codes SHOULD NOT be transmitted in the clear.

The "state" and "scope" parameters SHOULD NOT include sensitive
client or resource owner information in plain text, as they can be
transmitted over insecure channels or stored insecurely.

### 10.9. Ensuring Endpoint Authenticity

In order to prevent man-in-the-middle attacks, the authorization
server MUST require the use of TLS with server authentication as
defined by [[RFC2818](https://tools.ietf.org/html/rfc2818)] for any request sent to the authorization and
token endpoints. The client MUST validate the authorization server's
TLS certificate as defined by [[RFC6125](https://tools.ietf.org/html/rfc6125)] and in accordance with its
requirements for server identity authentication.

### 10.10. Credentials-Guessing Attacks

The authorization server MUST prevent attackers from guessing access
tokens, authorization codes, refresh tokens, resource owner
passwords, and client credentials.

The probability of an attacker guessing generated tokens (and other
credentials not intended for handling by end-users) MUST be less than
or equal to 2^(-128) and SHOULD be less than or equal to 2^(-160).

The authorization server MUST utilize other means to protect
credentials intended for end-user usage.

### 10.11. Phishing Attacks

Wide deployment of this and similar protocols may cause end-users to
become inured to the practice of being redirected to websites where
they are asked to enter their passwords. If end-users are not
careful to verify the authenticity of these websites before entering
their credentials, it will be possible for attackers to exploit this
practice to steal resource owners' passwords.

Service providers should attempt to educate end-users about the risks
phishing attacks pose and should provide mechanisms that make it easy
for end-users to confirm the authenticity of their sites. Client
developers should consider the security implications of how they
interact with the user-agent (e.g., external, embedded), and the
ability of the end-user to verify the authenticity of the
authorization server.

To reduce the risk of phishing attacks, the authorization servers
MUST require the use of TLS on every endpoint used for end-user
interaction.

### 10.12. Cross-Site Request Forgery

Cross-site request forgery (CSRF) is an exploit in which an attacker
causes the user-agent of a victim end-user to follow a malicious URI
(e.g., provided to the user-agent as a misleading link, image, or
redirection) to a trusting server (usually established via the
presence of a valid session cookie).

A CSRF attack against the client's redirection URI allows an attacker
to inject its own authorization code or access token, which can
result in the client using an access token associated with the
attacker's protected resources rather than the victim's (e.g., save
the victim's bank account information to a protected resource
controlled by the attacker).

The client MUST implement CSRF protection for its redirection URI.
This is typically accomplished by requiring any request sent to the
redirection URI endpoint to include a value that binds the request to
the user-agent's authenticated state (e.g., a hash of the session
cookie used to authenticate the user-agent). The client SHOULD
utilize the "state" request parameter to deliver this value to the
authorization server when making an authorization request.

Once authorization has been obtained from the end-user, the
authorization server redirects the end-user's user-agent back to the
client with the required binding value contained in the "state"
parameter. The binding value enables the client to verify the
validity of the request by matching the binding value to the
user-agent's authenticated state. The binding value used for CSRF
protection MUST contain a non-guessable value (as described in
[Section 10.10](#1010-Credentials-Guessing-Attacks)), and the user-agent's authenticated state (e.g.,
session cookie, HTML5 local storage) MUST be kept in a location
accessible only to the client and the user-agent (i.e., protected by
same-origin policy).

A CSRF attack against the authorization server's authorization
endpoint can result in an attacker obtaining end-user authorization
for a malicious client without involving or alerting the end-user.

The authorization server MUST implement CSRF protection for its
authorization endpoint and ensure that a malicious client cannot
obtain authorization without the awareness and explicit consent of
the resource owner.

### 10.13. Clickjacking

In a clickjacking attack, an attacker registers a legitimate client
and then constructs a malicious site in which it loads the
authorization server's authorization endpoint web page in a
transparent iframe overlaid on top of a set of dummy buttons, which
are carefully constructed to be placed directly under important
buttons on the authorization page. When an end-user clicks a
misleading visible button, the end-user is actually clicking an
invisible button on the authorization page (such as an "Authorize"
button). This allows an attacker to trick a resource owner into
granting its client access without the end-user's knowledge.

To prevent this form of attack, native applications SHOULD use
external browsers instead of embedding browsers within the
application when requesting end-user authorization. For most newer
browsers, avoidance of iframes can be enforced by the authorization
server using the (non-standard) "x-frame-options" header. This
header can have two values, "deny" and "sameorigin", which will block
any framing, or framing by sites with a different origin,
respectively. For older browsers, JavaScript frame-busting
techniques can be used but may not be effective in all browsers.

### 10.14. Code Injection and Input Validation

A code injection attack occurs when an input or otherwise external
variable is used by an application unsanitized and causes
modification to the application logic. This may allow an attacker to
gain access to the application device or its data, cause denial of
service, or introduce a wide range of malicious side-effects.

The authorization server and client MUST sanitize (and validate when
possible) any value received -- in particular, the value of the
"state" and "redirect_uri" parameters.

### 10.15. Open Redirectors

The authorization server, authorization endpoint, and client
redirection endpoint can be improperly configured and operate as open
redirectors. An open redirector is an endpoint using a parameter to
automatically redirect a user-agent to the location specified by the
parameter value without any validation.

Open redirectors can be used in phishing attacks, or by an attacker
to get end-users to visit malicious sites by using the URI authority
component of a familiar and trusted destination. In addition, if the
authorization server allows the client to register only part of the
redirection URI, an attacker can use an open redirector operated by

the client to construct a redirection URI that will pass the
authorization server validation but will send the authorization code
or access token to an endpoint under the control of the attacker.

### 10.16. Misuse of Access Token to Impersonate Resource Owner in Implicit Flow

For public clients using implicit flows, this specification does not
provide any method for the client to determine what client an access
token was issued to.

A resource owner may willingly delegate access to a resource by
granting an access token to an attacker's malicious client. This may
be due to phishing or some other pretext. An attacker may also steal
a token via some other mechanism. An attacker may then attempt to
impersonate the resource owner by providing the access token to a
legitimate public client.

In the implicit flow (response_type=token), the attacker can easily
switch the token in the response from the authorization server,
replacing the real access token with the one previously issued to the
attacker.

Servers communicating with native applications that rely on being
passed an access token in the back channel to identify the user of
the client may be similarly compromised by an attacker creating a
compromised application that can inject arbitrary stolen access
tokens.

Any public client that makes the assumption that only the resource
owner can present it with a valid access token for the resource is
vulnerable to this type of attack.

This type of attack may expose information about the resource owner
at the legitimate client to the attacker (malicious client). This
will also allow the attacker to perform operations at the legitimate
client with the same permissions as the resource owner who originally
granted the access token or authorization code.

Authenticating resource owners to clients is out of scope for this
specification. Any specification that uses the authorization process
as a form of delegated end-user authentication to the client (e.g.,
third-party sign-in service) MUST NOT use the implicit flow without
additional security mechanisms that would enable the client to
determine if the access token was issued for its use (e.g., audience-
restricting the access token).

## 11. IANA Considerations

### 11.1. OAuth Access Token Types Registry

This specification establishes the OAuth Access Token Types registry.

Access token types are registered with a Specification Required
([[RFC5226](https://tools.ietf.org/html/rfc5226)]) after a two-week review period on the
oauth-ext-review@ietf.org mailing list, on the advice of one or more
Designated Experts. However, to allow for the allocation of values
prior to publication, the Designated Expert(s) may approve
registration once they are satisfied that such a specification will
be published.

Registration requests must be sent to the oauth-ext-review@ietf.org
mailing list for review and comment, with an appropriate subject
(e.g., "Request for access token type: example").

Within the review period, the Designated Expert(s) will either
approve or deny the registration request, communicating this decision
to the review list and IANA. Denials should include an explanation
and, if applicable, suggestions as to how to make the request
successful.

IANA must only accept registry updates from the Designated Expert(s)
and should direct all requests for registration to the review mailing
list.

#### 11.1.1. Registration Template

Type name:
The name requested (e.g., "example").

Additional Token Endpoint Response Parameters:
Additional response parameters returned together with the
"access_token" parameter. New parameters MUST be separately
registered in the OAuth Parameters registry as described by
[Section 11.2](#112-OAuth-Parameters-Registry).

HTTP Authentication Scheme(s):
The HTTP authentication scheme name(s), if any, used to
authenticate protected resource requests using access tokens of
this type.

Change controller:
For Standards Track RFCs, state "IETF". For others, give the name
of the responsible party. Other details (e.g., postal address,
email address, home page URI) may also be included.

Specification document(s):
Reference to the document(s) that specify the parameter,
preferably including a URI that can be used to retrieve a copy of
the document(s). An indication of the relevant sections may also
be included but is not required.

### 11.2. OAuth Parameters Registry

This specification establishes the OAuth Parameters registry.

Additional parameters for inclusion in the authorization endpoint
request, the authorization endpoint response, the token endpoint
request, or the token endpoint response are registered with a
Specification Required ([[RFC5226](https://tools.ietf.org/html/rfc5226)]) after a two-week review period on
the oauth-ext-review@ietf.org mailing list, on the advice of one or
more Designated Experts. However, to allow for the allocation of
values prior to publication, the Designated Expert(s) may approve
registration once they are satisfied that such a specification will
be published.

Registration requests must be sent to the oauth-ext-review@ietf.org
mailing list for review and comment, with an appropriate subject
(e.g., "Request for parameter: example").

Within the review period, the Designated Expert(s) will either
approve or deny the registration request, communicating this decision
to the review list and IANA. Denials should include an explanation
and, if applicable, suggestions as to how to make the request
successful.

IANA must only accept registry updates from the Designated Expert(s)
and should direct all requests for registration to the review mailing
list.

#### 11.2.1. Registration Template

Parameter name:
The name requested (e.g., "example").

Parameter usage location:
The location(s) where parameter can be used. The possible
locations are authorization request, authorization response, token
request, or token response.

Change controller:
For Standards Track RFCs, state "IETF". For others, give the name
of the responsible party. Other details (e.g., postal address,
email address, home page URI) may also be included.

Specification document(s):
Reference to the document(s) that specify the parameter,
preferably including a URI that can be used to retrieve a copy of
the document(s). An indication of the relevant sections may also
be included but is not required.

#### 11.2.2. Initial Registry Contents

The OAuth Parameters registry's initial contents are:

o Parameter name: client_id
o Parameter usage location: authorization request, token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: client_secret
o Parameter usage location: token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: response_type
o Parameter usage location: authorization request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: redirect_uri
o Parameter usage location: authorization request, token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: scope
o Parameter usage location: authorization request, authorization
response, token request, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: state
o Parameter usage location: authorization request, authorization
response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: code
o Parameter usage location: authorization response, token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: error_description
o Parameter usage location: authorization response, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: error_uri
o Parameter usage location: authorization response, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: grant_type
o Parameter usage location: token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: access_token
o Parameter usage location: authorization response, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: token_type
o Parameter usage location: authorization response, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: expires_in
o Parameter usage location: authorization response, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: username
o Parameter usage location: token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: password
o Parameter usage location: token request
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Parameter name: refresh_token
o Parameter usage location: token request, token response
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

### 11.3. OAuth Authorization Endpoint Response Types Registry

This specification establishes the OAuth Authorization Endpoint
Response Types registry.

Additional response types for use with the authorization endpoint are
registered with a Specification Required ([[RFC5226](https://tools.ietf.org/html/rfc5226)]) after a two-week
review period on the oauth-ext-review@ietf.org mailing list, on the
advice of one or more Designated Experts. However, to allow for the
allocation of values prior to publication, the Designated Expert(s)
may approve registration once they are satisfied that such a
specification will be published.

Registration requests must be sent to the oauth-ext-review@ietf.org
mailing list for review and comment, with an appropriate subject
(e.g., "Request for response type: example").

Within the review period, the Designated Expert(s) will either
approve or deny the registration request, communicating this decision
to the review list and IANA. Denials should include an explanation
and, if applicable, suggestions as to how to make the request
successful.

IANA must only accept registry updates from the Designated Expert(s)
and should direct all requests for registration to the review mailing
list.

#### 11.3.1. Registration Template

Response type name:
The name requested (e.g., "example").

Change controller:
For Standards Track RFCs, state "IETF". For others, give the name
of the responsible party. Other details (e.g., postal address,
email address, home page URI) may also be included.

Specification document(s):
Reference to the document(s) that specify the type, preferably
including a URI that can be used to retrieve a copy of the
document(s). An indication of the relevant sections may also be
included but is not required.

#### 11.3.2. Initial Registry Contents

The OAuth Authorization Endpoint Response Types registry's initial
contents are:

o Response type name: code
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

o Response type name: token
o Change controller: IETF
o Specification document(s): [RFC 6749](https://tools.ietf.org/html/rfc6749)

### 11.4. OAuth Extensions Error Registry

This specification establishes the OAuth Extensions Error registry.

Additional error codes used together with other protocol extensions
(i.e., extension grant types, access token types, or extension
parameters) are registered with a Specification Required ([[RFC5226](https://tools.ietf.org/html/rfc5226)])
after a two-week review period on the oauth-ext-review@ietf.org
mailing list, on the advice of one or more Designated Experts.
However, to allow for the allocation of values prior to publication,
the Designated Expert(s) may approve registration once they are
satisfied that such a specification will be published.

Registration requests must be sent to the oauth-ext-review@ietf.org
mailing list for review and comment, with an appropriate subject
(e.g., "Request for error code: example").

Within the review period, the Designated Expert(s) will either
approve or deny the registration request, communicating this decision
to the review list and IANA. Denials should include an explanation
and, if applicable, suggestions as to how to make the request
successful.

IANA must only accept registry updates from the Designated Expert(s)
and should direct all requests for registration to the review mailing
list.

#### 11.4.1. Registration Template

Error name:
The name requested (e.g., "example"). Values for the error name
MUST NOT include characters outside the set %x20-21 / %x23-5B /
%x5D-7E.

Error usage location:
The location(s) where the error can be used. The possible
locations are authorization code grant error response
([Section 4.1.2.1](#4.1.2.1.-Error-Response)), implicit grant error response
([Section 4.2.2.1](#4.2.2.1.-Error-Response)), token error response ([Section 5.2](#52-Error-Response)), or resource
access error response ([Section 7.2](#72-Error-Response)).

Related protocol extension:
The name of the extension grant type, access token type, or
extension parameter that the error code is used in conjunction
with.

Change controller:
For Standards Track RFCs, state "IETF". For others, give the name
of the responsible party. Other details (e.g., postal address,
email address, home page URI) may also be included.

Specification document(s):
Reference to the document(s) that specify the error code,
preferably including a URI that can be used to retrieve a copy of
the document(s). An indication of the relevant sections may also
be included but is not required.

## 12. References

### 12.1. Normative References

[RFC2119] Bradner, S., "Key words for use in RFCs to Indicate
Requirement Levels", BCP 14, RFC 2119, March 1997.

[RFC2246] Dierks, T. and C. Allen, "The TLS Protocol Version 1.0",
RFC 2246, January 1999.

[RFC2616] Fielding, R., Gettys, J., Mogul, J., Frystyk, H.,
Masinter, L., Leach, P., and T. Berners-Lee, "Hypertext
Transfer Protocol -- HTTP/1.1", RFC 2616, June 1999.

[RFC2617] Franks, J., Hallam-Baker, P., Hostetler, J., Lawrence, S.,
Leach, P., Luotonen, A., and L. Stewart, "HTTP
Authentication: Basic and Digest Access Authentication",
RFC 2617, June 1999.

[RFC2818] Rescorla, E., "HTTP Over TLS", RFC 2818, May 2000.

[RFC3629] Yergeau, F., "UTF-8, a transformation format of
ISO 10646", STD 63, RFC 3629, November 2003.

[RFC3986] Berners-Lee, T., Fielding, R., and L. Masinter, "Uniform
Resource Identifier (URI): Generic Syntax", STD 66,
RFC 3986, January 2005.

[RFC4627] Crockford, D., "The application/json Media Type for
JavaScript Object Notation (JSON)", RFC 4627, July 2006.

[RFC4949] Shirey, R., "Internet Security Glossary, Version 2",
RFC 4949, August 2007.

[RFC5226] Narten, T. and H. Alvestrand, "Guidelines for Writing an
IANA Considerations Section in RFCs", BCP 26, RFC 5226,
May 2008.

[RFC5234] Crocker, D. and P. Overell, "Augmented BNF for Syntax
Specifications: ABNF", STD 68, RFC 5234, January 2008.

[RFC5246] Dierks, T. and E. Rescorla, "The Transport Layer Security
(TLS) Protocol Version 1.2", RFC 5246, August 2008.

[RFC6125] Saint-Andre, P. and J. Hodges, "Representation and
Verification of Domain-Based Application Service Identity
within Internet Public Key Infrastructure Using X.509
(PKIX) Certificates in the Context of Transport Layer
Security (TLS)", RFC 6125, March 2011.

[USASCII] American National Standards Institute, "Coded Character
Set -- 7-bit American Standard Code for Information
Interchange", ANSI X3.4, 1986.

[W3C.REC-html401-19991224]
Raggett, D., Le Hors, A., and I. Jacobs, "HTML 4.01
Specification", World Wide Web Consortium
Recommendation REC-html401-19991224, December 1999,
<http://www.w3.org/TR/1999/REC-html401-19991224>.

[W3C.REC-xml-20081126]
Bray, T., Paoli, J., Sperberg-McQueen, C., Maler, E.,
and F. Yergeau, "Extensible Markup Language (XML) 1.0
(Fifth Edition)", World Wide Web Consortium
Recommendation REC-xml-20081126, November 2008,
<http://www.w3.org/TR/2008/REC-xml-20081126>.

Hardt Standards Track [Page 69]

RFC 6749 OAuth 2.0 October 2012

### 12.2. Informative References

[OAuth-HTTP-MAC]
Hammer-Lahav, E., Ed., "HTTP Authentication: MAC Access
Authentication", Work in Progress, February 2012.

[OAuth-SAML2]
Campbell, B. and C. Mortimore, "SAML 2.0 Bearer Assertion
Profiles for OAuth 2.0", Work in Progress, September 2012.

[OAuth-THREATMODEL]
Lodderstedt, T., Ed., McGloin, M., and P. Hunt, "OAuth 2.0
Threat Model and Security Considerations", Work
in Progress, October 2012.

[OAuth-WRAP]
Hardt, D., Ed., Tom, A., Eaton, B., and Y. Goland, "OAuth
Web Resource Authorization Profiles", Work in Progress,
January 2010.

[RFC5849] Hammer-Lahav, E., "The OAuth 1.0 Protocol", RFC 5849,
April 2010.

[RFC6750] Jones, M. and D. Hardt, "The OAuth 2.0 Authorization
Framework: Bearer Token Usage", RFC 6750, October 2012.

Hardt Standards Track [Page 70]

RFC 6749 OAuth 2.0 October 2012

# Appendix A. Augmented Backus-Naur Form (ABNF) Syntax

This section provides Augmented Backus-Naur Form (ABNF) syntax
descriptions for the elements defined in this specification using the
notation of [RFC5234]. The ABNF below is defined in terms of Unicode
code points [W3C.REC-xml-20081126]; these characters are typically
encoded in UTF-8. Elements are presented in the order first defined.

Some of the definitions that follow use the "URI-reference"
definition from [RFC3986].

Some of the definitions that follow use these common definitions:

     VSCHAR     = %x20-7E
     NQCHAR     = %x21 / %x23-5B / %x5D-7E
     NQSCHAR    = %x20-21 / %x23-5B / %x5D-7E
     UNICODECHARNOCRLF = %x09 /%x20-7E / %x80-D7FF /
                         %xE000-FFFD / %x10000-10FFFF

(The UNICODECHARNOCRLF definition is based upon the Char definition
in Section 2.2 of [W3C.REC-xml-20081126], but omitting the Carriage
Return and Linefeed characters.)

## A.1. "client_id" Syntax

The "client_id" element is defined in Section 2.3.1:

     client-id     = *VSCHAR

## A.2. "client_secret" Syntax

The "client_secret" element is defined in Section 2.3.1:

     client-secret = *VSCHAR

## A.3. "response_type" Syntax

The "response_type" element is defined in Sections 3.1.1 and 8.4:

     response-type = response-name *( SP response-name )
     response-name = 1*response-char
     response-char = "_" / DIGIT / ALPHA

Hardt Standards Track [Page 71]

RFC 6749 OAuth 2.0 October 2012

## A.4. "scope" Syntax

The "scope" element is defined in Section 3.3:

     scope       = scope-token *( SP scope-token )
     scope-token = 1*NQCHAR

## A.5. "state" Syntax

The "state" element is defined in Sections 4.1.1, 4.1.2, 4.1.2.1,
4.2.1, 4.2.2, and 4.2.2.1:

     state      = 1*VSCHAR

## A.6. "redirect_uri" Syntax

The "redirect_uri" element is defined in Sections 4.1.1, 4.1.3,
and 4.2.1:

     redirect-uri      = URI-reference

## A.7. "error" Syntax

The "error" element is defined in Sections 4.1.2.1, 4.2.2.1, 5.2,
7.2, and 8.5:

     error             = 1*NQSCHAR

## A.8. "error_description" Syntax

The "error_description" element is defined in Sections 4.1.2.1,
4.2.2.1, 5.2, and 7.2:

     error-description = 1*NQSCHAR

## A.9. "error_uri" Syntax

The "error_uri" element is defined in Sections 4.1.2.1, 4.2.2.1, 5.2,
and 7.2:

     error-uri         = URI-reference

Hardt Standards Track [Page 72]

RFC 6749 OAuth 2.0 October 2012

## A.10. "grant_type" Syntax

The "grant_type" element is defined in Sections 4.1.3, 4.3.2, 4.4.2,
4.5, and 6:

     grant-type = grant-name / URI-reference
     grant-name = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

## A.11. "code" Syntax

The "code" element is defined in Section 4.1.3:

     code       = 1*VSCHAR

## A.12. "access_token" Syntax

The "access_token" element is defined in Sections 4.2.2 and 5.1:

     access-token = 1*VSCHAR

## A.13. "token_type" Syntax

The "token_type" element is defined in Sections 4.2.2, 5.1, and 8.1:

     token-type = type-name / URI-reference
     type-name  = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

## A.14. "expires_in" Syntax

The "expires_in" element is defined in Sections 4.2.2 and 5.1:

     expires-in = 1*DIGIT

## A.15. "username" Syntax

The "username" element is defined in Section 4.3.2:

     username = *UNICODECHARNOCRLF

## A.16. "password" Syntax

The "password" element is defined in Section 4.3.2:

     password = *UNICODECHARNOCRLF

Hardt Standards Track [Page 73]

RFC 6749 OAuth 2.0 October 2012

## A.17. "refresh_token" Syntax

The "refresh_token" element is defined in Sections 5.1 and 6:

     refresh-token = 1*VSCHAR

## A.18. Endpoint Parameter Syntax

The syntax for new endpoint parameters is defined in Section 8.2:

     param-name = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

# Appendix B. Use of application/x-www-form-urlencoded Media Type

At the time of publication of this specification, the
"application/x-www-form-urlencoded" media type was defined in
Section 17.13.4 of [W3C.REC-html401-19991224] but not registered in
the IANA MIME Media Types registry
(<http://www.iana.org/assignments/media-types>). Furthermore, that
definition is incomplete, as it does not consider non-US-ASCII
characters.

To address this shortcoming when generating payloads using this media
type, names and values MUST be encoded using the UTF-8 character
encoding scheme [RFC3629] first; the resulting octet sequence then
needs to be further encoded using the escaping rules defined in
[W3C.REC-html401-19991224].

When parsing data from a payload using this media type, the names and
values resulting from reversing the name/value encoding consequently
need to be treated as octet sequences, to be decoded using the UTF-8
character encoding scheme.

For example, the value consisting of the six Unicode code points
(1) U+0020 (SPACE), (2) U+0025 (PERCENT SIGN),
(3) U+0026 (AMPERSAND), (4) U+002B (PLUS SIGN),
(5) U+00A3 (POUND SIGN), and (6) U+20AC (EURO SIGN) would be encoded
into the octet sequence below (using hexadecimal notation):

     20 25 26 2B C2 A3 E2 82 AC

and then represented in the payload as:

     +%25%26%2B%C2%A3%E2%82%AC
