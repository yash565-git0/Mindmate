# Software Requirements Specification (SRS)

**MindMate – AI Mood Companion App**
Version 1.0
Date: 13 August 2025
Authors: MindMate Development Team

***

## Revision History

| Version | Date | Description | Author |
| :-- | :-- | :-- | :-- |
| 1.0 | 13-08-2025 | Initial draft | MindMate Dev Team |


***

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [Specific Requirements](#3-specific-requirements)
4. [Appendices](#4-appendices)
5. [Index](#5-index)

***

## 1. Introduction

### 1.1 Purpose

The purpose of this Software Requirements Specification (SRS) document is to define the requirements for **MindMate**, a cross-platform application that analyzes a user's daily text or voice input to determine mood and deliver context-appropriate recommendations such as inspirational quotes, music, or calming exercises.

### 1.2 Scope

MindMate enables users to record their daily reflections through text or voice. The system performs sentiment and emotion analysis using third-party APIs or lightweight on-device models, stores mood data, and responds with tailored well-being content. A history tracker and motivational streak system encourage continued engagement and self-reflection.

### 1.3 Definitions, Acronyms, and Abbreviations

| Term | Definition |
| :-- | :-- |
| **API** | Application Programming Interface |
| **NLP** | Natural Language Processing |
| **ML** | Machine Learning |
| **FR** | Functional Requirement |
| **NFR** | Non-Functional Requirement |
| **STT** | Speech-to-Text |
| **SRS** | Software Requirements Specification |

### 1.4 References

1. IEEE Std 830-1998, "Recommended Practice for Software Requirements Specifications".
2. Arya.ai, "7 Best Sentiment Analysis APIs" (2025).
3. Google Cloud, "Speech-to-Text API Docs" (2025).
4. Onix-Systems, "How to Develop a Mood Tracker App" (2025).

### 1.5 Overview

The remainder of this document details the overall product features, system environment, external interfaces, and precise functional and non-functional requirements.

***

## 2. Overall Description

### 2.1 Product Perspective

MindMate is a standalone mobile and web application built with a React/React Native front end, Node.js backend, and cloud-hosted PostgreSQL database. Sentiment analysis and speech processing leverage managed AI services. The app interfaces with third-party content providers (quotes, music streaming) to curate recommendations.

### 2.2 Product Functions

- Capture daily text or voice journals.
- Convert speech input to text.
- Analyze sentiment and emotion from user input.
- Generate context-sensitive recommendations.
- Display chat-style responses.
- Visualize mood trends in a calendar or graph.
- Maintain motivational streaks based on consecutive days of entry.
- Allow secure authentication and profile management.


### 2.3 User Classes and Characteristics

| User Class | Description | Technical Expertise |
| :-- | :-- | :-- |
| **End User** | Individuals aged 13+ seeking mood insights | Basic smartphone/web skills |
| **Administrator** | Maintains content libraries and monitors system health | Technical background |

### 2.4 Operating Environment

- iOS 15+ / Android 12+ via React Native.
- Modern web browsers (Chrome, Edge, Safari, Firefox).
- Node.js 20 LTS runtime on cloud (AWS/GCP/Azure).
- PostgreSQL 15 database.
- REST/GraphQL API over HTTPS.
- External AI services (e.g., Google Cloud Natural Language, OpenAI Emotion).


### 2.5 Design and Implementation Constraints

- Must comply with GDPR and relevant data-privacy regulations.
- Open-source libraries must carry permissive licenses (MIT/Apache 2).
- Offline mode limited to journaling; AI requires internet connectivity.
- Maximum response latency ≤ 2 s for 95th percentile.


### 2.6 User Documentation

- In-app onboarding tutorial.
- FAQ \& Help Center web page.
- Administrator handbook (PDF).


### 2.7 Assumptions and Dependencies

- Reliable third-party sentiment and speech APIs remain available.
- Users grant microphone and storage permissions.
- Music recommendations require integration with Spotify/Apple Music APIs.

***

## 3. Specific Requirements

### 3.1 External Interface Requirements

#### 3.1.1 User Interfaces

- Responsive UI with chat bubble layout.
- Voice input button supports press-and-hold recording.
- Dashboard showing weekly mood graph and streak counter.
- Settings page for notification preferences and data export.


#### 3.1.2 Hardware Interfaces

- Mobile device microphone for voice capture.
- Optional haptic engine for feedback on mood detection.


#### 3.1.3 Software Interfaces

| Interface | Protocol | Description |
| :-- | :-- | :-- |
| **STT API** | REST/JSON | Uploads audio blob, returns transcript |
| **Sentiment API** | REST/JSON | Submits text, returns mood score \& emotion label |
| **Quotes API** | REST/JSON | Retrieves quote by emotion category |
| **Music API** | OAuth 2.0 | Searches playlists matching mood |

#### 3.1.4 Communications Interfaces

- All external calls over HTTPS (TLS 1.3).
- Push notifications via Firebase Cloud Messaging/APNs.


### 3.2 Functional Requirements

| ID | Description | Priority |
| :-- | :-- | :-- |
| **FR-1** | The system SHALL record textual journal entries. | High |
| **FR-2** | The system SHALL convert voice input to text using STT API. | High |
| **FR-3** | The system SHALL analyze sentiment and emotion of input text. | High |
| **FR-4** | The system SHALL generate a recommendation (quote/song/activity) appropriate to detected mood. | High |
| **FR-5** | The system SHALL store each mood entry in the user's timeline. | High |
| **FR-6** | The system SHALL display a visual mood history with filters (day/week/month). | Medium |
| **FR-7** | The system SHALL calculate and display motivational streaks based on consecutive days of entry. | Medium |
| **FR-8** | The system SHALL allow users to register, log in, and reset passwords securely. | High |
| **FR-9** | The system SHALL let users export their data in JSON or CSV format. | Low |

### 3.3 Non-Functional Requirements

| Category | Requirement |
| :-- | :-- |
| **Performance** | 95% of sentiment analysis responses < 1500 ms. |
| **Reliability** | 99.5% uptime monthly. |
| **Security** | All data encrypted in transit (TLS) and at rest (AES-256). Access controlled via JWT tokens. |
| **Privacy** | Collect only minimal PII; provide data deletion and consent management. |
| **Usability** | First-time users complete entry flow in ≤ 3 steps. |
| **Maintainability** | Codebase follows ESLint and Prettier standards with ≥ 80% unit test coverage. |
| **Portability** | Source supports web and mobile builds from a common codebase. |
| **Accessibility** | Conforms to WCAG 2.1 AA guidelines. |

### 3.4 Logical Database Requirements

Entity-relationship overview:

* **User** (*user_id*, name, email, password_hash, signup_date)
* **MoodEntry** (*entry_id*, user_id, timestamp, text, mood_score, emotion_label)
* **Recommendation** (*rec_id*, entry_id, type, content, source_url)
* **Streak** (*user_id*, current_streak, longest_streak, updated_at)


### 3.5 System Features (Use Case-Driven)

#### 3.5.1 Record Daily Mood

**Actors:** End User
**Pre-condition:** Authenticated session.
**Main Flow:** 1) User inputs text/voice → 2) System transcribes (if voice) → 3) System analyzes sentiment → 4) System stores result → 5) System responds with recommendation.
**Post-condition:** MoodEntry persisted.

#### 3.5.2 View Mood History

**Actors:** End User
**Pre-condition:** Existing MoodEntries.
**Main Flow:** User opens dashboard and selects timeframe; system renders chart and streak.

#### 3.5.3 Admin Content Management

**Actors:** Administrator
**Main Flow:** Admin logs in to CMS, creates or edits quotes and activities tagged by emotion.

### 3.6 Software System Attributes

- **Availability:** Deployed across two availability zones with auto-scaling.
- **Serviceability:** Cloud monitoring and centralized logging (Grafana/Prometheus).
- **Scalability:** Horizontal scaling in Kubernetes cluster to 10,000 DAU.
- **Compliance:** GDPR Article 17 right to erasure implemented via asynchronous job.

***

## 4. Appendices

### Appendix A – Glossary

* **Emotion Label:** Categorical value such as *Happy*, *Sad*, *Angry*, *Calm*, derived from sentiment API.
* **Motivational Streak:** Consecutive days with at least one completed mood entry.


### Appendix B – Analysis Models (Textual)

- **Sequence Diagram:** *Record Daily Mood* – User → App → STT API → Sentiment API → Recommendation Engine → Database → User.
- **Data Flow:** User input → Processing → Storage → Dashboard Visualization.

***

## 5. Index

*Authentication*, *Emotion*, *MoodEntry*, *Recommendation*, *Sentiment*, *Streak*

