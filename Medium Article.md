---
source: https://shahbhat.medium.com/building-a-decentralized-messaging-with-end-to-end-encryption-using-open-standards-5e8f6229730e
---

# Building A Decentralized Messaging with End-to-End Encryption using Open Standards

# Abstract

A number of messaging apps such as [FB Messenger](https://www.messenger.com/), [Telegram](https://telegram.org/), [Matrix Element](https://element.io/), [Signal](https://www.signal.org/), [Status.im](https://status.im/), [Threema](https://threema.ch/en/), [Whatsapp](https://www.whatsapp.com/), etc. offer an end-to-end encryption messaging, however they all use proprietary APIs for message storage/relaying, contacts discovery, keys management, group-administration and other services. These centralized APIs can be used to extract metadata from messages, to build social graph from the contacts or to forge message order/acknowledgements or group membership with malicious access. Though, most of these apps support forward secrecy and post-compromise security (PCS) for pairwise communication but some discard PCS guarantees in groups communication and incur high cost for updating users’ keys with large groups. This paper reviews popular messaging apps for their capabilities and security profile. It explores industry standards for open messaging protocols that can be used to build a secured, decentralized and scalable messaging system with end-to-end encryption and post-compromise security guarantees for both pairwise and group communication. It then proposes an architecture with open standards and decentralized building blocks such as identity, keys management, storage and messaging protocol that can be used to build a trustless and secured messaging system. Lastly, this paper shows how this decentralized architecture can be used to build other high-level decentralized applications for communication, collaboration, payments, social platforms, etc.

Note: This paper only examines text messaging between two parties or among groups and other features such as public channels, bots, payments, WebRTC, audio and video chat are out of scope for this discussion.

# Messaging Apps

Following section surveys popular messaging apps and evaluates their strengths and weaknesses:

# Survey of Popular Messaging Apps

## Briar

Briar is a decentralized messaging app with end-to-end encryption that is designed for activists, journalists and general public. It can sync messages via a variety of network protocols such as Bluetooth, Wi-Fi or Tor [[51](https://briarproject.org/how-it-works/)]. The messages are encrypted using Bramble Transport Protocol ([BTP](https://code.briarproject.org/briar/briar-spec/blob/master/protocols/BTP.md)) that supports confidentiality, integrity, authenticity, and forward secrecy. BTP establishes a root key between two parties and derives a temporary key for encrypting communication. The temporary key is rotated periodically based on time periods. It supports group communication by defining a group as a label that can subscribe to a channel similar to Usenet.

## Facebook Messenger

Facebook Messenger is a closed-source messaging app that uses Signal protocol for end-to-end encryption. Facebook uses centralized authentication and delivery services for message routing. Its privacy policies are quite appalling and per Apple’s privacy polices, it collects a lot of metadata from users such as [[59](https://www.techradar.com/news/apples-privacy-labels-reveals-whatsapp-and-facebook-messengers-hunger-for-user-data)]:

- Purchase History
- Other Financial Info
- Precise Location
- Coarse Location
- Physical Address
- Email Address
- Name
- Phone Number
- Other User Contact Info
- Contacts
- Photos or Videos
- Gameplay Content
- Other User Content
- Search History
- Browsing History
- User ID
- Device ID
- Product Interaction
- Advertising Data
- Other Usage Data
- Crash Data
- Performance Data
- Other Diagnostic Data
- Other Data Types
- Browsing History
- Health
- Fitness
- Payment Info
- Photos or Videos
- Audio Data
- Gameplay Content
- Customer Support
- Other User Content
- Search History
- Sensitive Info
- iMessage
- Email address
- Phone number Search history
- Device ID

The IOS Messenger disables App Protection (ATS) that can lead to insecure network connections and uses audio, fetch, location, remote-notification and void modes in background. The Android Messenger apps uses smaller than 1024 bits for signing the app and allows dynamic code loading that can be used to inject malicious code. The Android app also uses `[addJavaScriptInterface()](https://developer.android.com/training/articles/security-tips?hl=fr#WebView)` that allows calling native operations from Javascript that can be used for malicious access.

## Session App

Session is an open source application that uses decentralized infrastructure for messaging based on decentralized storage and onion routing protocol to send end-to-end encrypted messages. It originally used [Signal protocol](https://www.signal.org/docs/) to provide end-to-end encryption for pairwise communication and Sender Keys for group communication. However, Session recently updated its messaging app to use Session protocol instead of Signal protocol that is backward compatible with Signal protocol and provides similar guarantees for end-to-end encryption. Session guarantees Perfect Forward Secrecy (PFS) and deniability, which are further strengthened by using anonymous accounts and disappearing messages. The new Session protocol uses a long-term shared key instead of sender-key for group communication that are shared via pairwise channels and are recreated when a user leaves the group. This also helps better support for multiple devices that shares user’s long-term keypair to a new device and duplicates sent messages into their own swarm [[57](https://getsession.org/session-protocol-technical-information/)].

Session reduces metadata collection by using X25519 public/private keypair as identity as opposed to using phone number and using decentralized network of nodes for routing messages, which use onion protocol to hide IP-addresses of users. Session uses a network of Loki Service Node based on Loki blockchain, which itself is based on the Cryptonote protocol [[20](https://cryptonote.org/whitepaper.pdf)]. It uses proof-of-stake to prevent against Sybil attack and rewards service-node providers with Loke cryptocurrency that helps create a sustainable network as opposed to public networks such as Tor or I2P. The storage provided by a subset of swarm nodes on this network can be used for storing encrypted-messages when recipients are offline or sharing encrypted attachments with other members of the group. As session app does not use any centralized services, it shares prekey bundles via friend request and adds additional metadata to each message for routing purpose. Also, it uses Loki Name Service (LNS) to map keypair based identity with a short text or username that can be used to share identity with friends [[42](https://itsfoss.com/session-messenger/), [57](https://getsession.org/session-protocol-technical-information/)].

**Mobile Clients**

The IOS Session app uses CommonCrypto API, Sodium, Curve25519Kit and SessionProtocolKit frameworks for crypto/DR primitivies and includes fetch and remote-notification in background mode. The IOS app also stores sensitive encryption key data in Sqlite database, which is not protected with [_NSFileProtectionComplete_](https://support.apple.com/guide/security/data-protection-classes-secb010e978a/web) or [_NSFileProtectionCompleteUntilFirstUserAuthentication_](https://support.apple.com/guide/security/data-protection-classes-secb010e978a/web) flags.

## Signal

Signal is an open source messaging app that uses Curve25519, HMAC-SHA256, X3DH and Double Ratchet algorithm to support end-to-end encryption and forward secrecy. It uses phone number as identity and Signal server generates a random password upon installation, which is sent to Signal server with each request. It supports both direct communication between two users and group communication with multiple members.

A group message in Signal is treated as a direct message but with 128-bit group-id. The Signal app sends a message for each member of the group to Signal server for delivery. The server forwards the message and acknowledges messages from sender. The recipient acknowledges receipt to Signal server. The acknowledgements do not use end-to-end encryption [[5](https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/), [18](https://eprint.iacr.org/2017/713.pdf), [32](https://blog.cryptographyengineering.com/2018/01/10/attack-of-the-week-group-messaging-in-whatsapp-and-signal/)]. Signal doesn’t enforce any access control for group management and any user can add members and it does not allow any user to remove other members but a member can remove herself from the group. Due to lack of any access control and validation of group membership by invitee, a malicious user can add a member and eavesdrop messages if he acquires a member’s phone number and group id. The server acknowledges messages from sender and receivers acknowledges the receipt to server but these acknowledgements are susceptible to forging. As all messages are routed through Signal server containing plaintext metadata such as sender, receiver, timestamps, IP-addresses, etc, a malicious user with access to the server can change timestamps thus affecting message order or collect metadata about relationships among users [[4](https://architecture.messaginglayersecurity.rocks/), [18](https://eprint.iacr.org/2017/713.pdf), [48](https://medium.com/tenable-techblog/turning-signal-app-into-a-coarse-tracking-device-643eb4298447), [83](https://dessalines.github.io/essays/why_not_signal.html)].

Signal app recently added a feature “Secured Value Recovery” to upload contact list without being accessible by Signal. Signal uses Intel SGX (Software Guard Extension) to enclave data processing. However, Signal uses a low-entropy 4-digit PIN for encryption that is susceptible to dictionary attacks. Also, SGX is vulnerable to “speculative execution” side channels attacks, which can allow an attacker to extract secrets from SGX [[16](https://arxiv.org/pdf/2006.13353.pdf), [17](https://sgaxe.com/files/SGAxe.pdf)].

> _Wow. SGX completely broken, again._ [_https://t.co/YqNNJd3yL3_](https://t.co/YqNNJd3yL3)
> 
> _— Matthew Green (@matthew_d_green)_ [_June 9, 2020_](https://twitter.com/matthew_d_green/status/1270406251063762944?ref_src=twsrc%5Etfw)

Signal also uses dark pattern to automatically opt users into this cloud backup by asking users to generate PIN code without explaining the ramifications of their actions [[27](https://blog.cryptographyengineering.com/2020/07/10/a-few-thoughts-about-signals-secure-value-recovery/), [30](https://nakedsecurity.sophos.com/2020/05/22/signal-secure-messaging-can-now-identify-you-without-a-phone-number/)]. Though, Signal receives favorable coverage in tech world, but it suffers from WhatsApp envy and it tends to copy dubious features from WhatsApp instead of building a trustless security and federated delivery services like email for secured communication [[52](https://drewdevault.com/2018/08/08/Signal.html)]. Instead, Moxie has actively tried to stop other open source projects such as [LibreSignal](https://github.com/LibreSignal/LibreSignal/issues/37#issuecomment-217211165) to use their servers [[60](https://www.oyd.org.tr/en/articles/signal/)]. Moxie also has declined collaboration with open messaging specifications such as [Matrix](https://matrix.org/) [[63](https://matrix.org/blog/2020/01/02/on-privacy-versus-freedom)]. The open source nature of Signal is also questionable because their server code hasn’t been updated for almost a year and it’s unclear what changes they have added locally [[78](https://lemmy.ml/post/55595.), [83](https://dessalines.github.io/essays/why_not_signal.html)].

**Mobile Clients**

The IOS Signal app uses audio, fetch, remote-notification and void modes in background. The Android Signal app uses smaller than 1024 bit key for signing the app and allows dynamic code loading that can lead to malicious code injection. The Android Manifest file does not protect broadcast receivers with permissions such as Signature/SignatureorSystem that can result in data leak.

## Status.im

Status is an open source messaging platform and Web 3.0 browser based on Ethereum Network and Status Network Token (STN) utility crypto-token. Status supports confidentiality, authenticity, end-to-end encryption and forward secrecy. The end-to-end encryption is implented using Double Ratchet protocol and [X3DH](https://signal.org/docs/specifications/x3dh/) based prekeys. The cryptographic primitives include Curve25519, AES-256-GCM, KECCAK-256, SECP256K1, ECDSA, ECIES and AES-256-CTR with HMAC-SHA-256 [[55](https://status.im/technical/pfs.html)]. Status uses [Swarm](https://swarm.ethereum.org/) for decentralized storage and [Whisper](https://geth.ethereum.org/docs/whisper/how-to-whisper) for peer-to-peer communication. It has recently switched to [Waku](https://specs.status.im/spec/10) for peer-to-peer communication for better scalability, spam-resistance and support for libp2p. It uses a modified Signal protocol to suit decentralized infrastructure for end-to-end encryption and mobile app also supports payments and crypto asset wallet. The users are identified by [SECP256k1](https://github.com/ethereum/EIPs/pull/619) public key and STN token holders can regiser their usernames to Ehereum Name Service (ENS) to make readable and recoverable access point. As Status doesn’t use phone number or email for identity, it provides better privacy and protection against censorship. Status lacks self-destructing/disappearing messages, audio and video calls. As push notifications generally requires central services, Status supports notifications while the application is running but with [Whisper V5 protocol](https://github.com/status-im/status-go/wiki/Whisper-Push-Notifications) it can store offline inbox, which can also be used for decentralized push notification [[43](https://status.im/whitepaper.pdf), [44](https://our.status.im/peer-to-peer-messaging-where-whisper-falls-short-and-waku-picks-up/), [46](https://status.im/research/secure_messaging_compendium.html)].

## Telegram

Telegram is a closed-source messaging app that was created by Pavel Durov. Telegram uses a in-house encryption protocol as opposed to Signal protocol in most other apps, thus it has not been vetted with comprehensive security analysis. It has been found to be lacking security and privacy that it claims, e.g. an investigation by Vice found that Telegram doesn’t enable end-to-end encryption by default. It also stores chat messages in cloud that can be spied by government or spy agencies. Telegram copies all contacts from your phone to their servers for matching other Telegram users. As all outgoing messages are routed through Telegram server, it collects metadata including IP-addresses, device profile, etc that can be used to track users. Telegram was also found to be exposing users’ locations when viewing nearby users [[25](https://blog.ahmed.nyc/2021/01/if-you-use-this-feature-on-telegram.html), [26](https://www.vice.com/en/article/jgqqv8/five-reasons-you-should-delete-telegram-from-your-phone), [40](https://www.nytimes.com/2019/06/13/world/asia/hong-kong-telegram-protests.html)]. Researches also have found “crime-pizza” vulnerability where attacker could alter the order of messages coming from a client to the telegram server in cloud [[81](https://www.cyberscoop.com/telegram-app-security-encryption/)]. Due to its use of customized encryption protocol and implementation, severe security bugs have been found in Telegram [[62](https://buttondown.email/cryptography-dispatches/archive/cryptography-dispatches-the-most-backdoor-looking/), [72](https://cqi.inf.usi.ch/publications/telegram_vs_signal.pdf)].

**Mobile Clients**

The IOS Telegram app does not use App Protection (ATS) that can lead to insecure network connections and uses deprecated and insecure methods [unarchiveObjectWithData](https://developer.apple.com/documentation/foundation/nskeyedunarchiver/1413894-unarchiveobjectwithdata) / [unarchiveObjectWithFile](https://developer.apple.com/documentation/foundation/nskeyedunarchiver/1417153-unarchiveobjectwithfile) for deserialization instead of [decodeObjectForKey](https://developer.apple.com/documentation/foundation/nskeyedunarchiver/1409082-decodeobjectforkey) / [decodeObjectOfClass](https://developer.apple.com/documentation/foundation/nscoder/1442558-decodeobjectofclass). It uses audio, fetch, location, remote-notification and voip modes in background. The Android app uses hard coded API keys including Google Maps and Android Manifest file does not protect broadcast receivers with permissions such as _Signature_/_SignatureorSystem_ that can result in data leak.

## Threema

Threema started as closed-source but it [open sourced](https://threema.ch/en/open-source) its messaging app recently in December 2020 under [AGPLv3](https://en.wikipedia.org/wiki/GNU_Affero_General_Public_License). Threema messaging app uses federated servers to deliver messages and distribute user keys but they don’t provide relaying messages from server to another. Threema uses a variation of Signal protocol that uses Diffie-Hellman key exchanges (DHKEs) using Curve25519, implements end-to-end encryption using XSalsa20 cipher and validates integrity using Poly1305-AES MAC. Threema limits the maximum members in a group to 50 and allows only creator to manage group membership. All group communication uses same protocol as pairwise communication except group messages are not end-to-end acknowledged. However, all group communication reuses same DHKE of their long term public keys, thus it does not provide forward secrecy. Threema orders messages by received data similar to Signal and WhatsApp, which can be forged by someone with access to Threema server [[18](https://eprint.iacr.org/2017/713.pdf)].

## WhatsApp

WhatsApp is a closed source messaging app that uses Signal protocol for pairwise communication and uses variation of sender-keys for group communication. WhatsApp uses Noise Pipes based on 128-bit Curve25519, AES-GCM and SHA256 to protect communication between client and server [[38](https://www.whatsapp.com/security/)]. WhatsApp uses libsignal-protocol-c library that uses Signal key exchange protocol based on X3DH and Double Ratchet algorithm to communicate among users [[37](https://medium.com/@schirrmacher/analyzing-whatsapp-calls-176a9e776213)]. A group contains a unique id, a set of admins and members with max limit of 256 members. Each user generates sender-key upon joining the group that are shared with other members using pairwise channels. The group communication does not provide future secrecy as sender-keys are reused for encrypting group communication. The group’s sender-keys are rotated when a member is removed otherwise a compromised member can passively read future messages [[5](https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/)]. WhatsApp uses centralized servers for all group management instead of using cryptography to protect integrity of group members. As a result, server may add a member to eavesdrop on the conversation [[18](https://eprint.iacr.org/2017/713.pdf), [32](https://blog.cryptographyengineering.com/2018/01/10/attack-of-the-week-group-messaging-in-whatsapp-and-signal/), [35](https://i.blackhat.com/USA-19/Wednesday/us-19-Zaikin-Reverse-Engineering-WhatsApp-Encryption-For-Chat-Manipulation-And-More.pdf)]. All outgoing messages are routed through WhatsApp server that can collect metadata, forge acknowledgements or change order of the messages. WhatsApp uses a mobile phone to identify each user and supports multiple devices per user by connecting secondary devices via QR code. The incoming message is first received by primary device and it then shares message with secondary devices using pairwise channel. The secondary devices cannot receive messages if the primary device is offline [[2](https://research.fb.com/wp-content/uploads/2018/11/On-Ends-to-Ends-Encryption-Asynchronous-Group-Messaging-with-Strong-Security-Guarantees.pdf), [18](https://eprint.iacr.org/2017/713.pdf)].

As Facebook now owns WhatsApp, it now mandates sharing all metadata with Facebook such as phone numbers, contacts, profile names, profile pictures, status messages, diagnostic data, etc from phone [[22](https://www.xda-developers.com/whatsapp-updates-terms-privacy-policy-mandate-data-sharing-facebook/), [31](https://arstechnica.com/tech-policy/2021/01/whatsapp-users-must-share-their-data-with-facebook-or-stop-using-the-app/)]. Due to widespread use of WhatsApp, a lot security companies now provide hacking tools to governments to monitor journalists or activists as reported on [[28](https://androidrookies.com/german-police-can-access-any-whatsapp-message-without-any-malware/), [29](https://www.theguardian.com/technology/2020/jul/02/whatsapp-groups-conspiracy-theories-disinformation-democracy), [45](https://www.wired.com/story/whatsapp-security-flaws-encryption-group-chats/), [47](https://www.dw.com/en/germanys-data-chief-tells-ministries-whatsapp-is-a-no-go/a-53474413), [54](https://citizenlab.ca/2020/12/the-great-ipwn-journalists-hacked-with-suspected-nso-group-imessage-zero-click-exploit/)]. Apple’s privacy policies for WhatsApp now includes all metadata collected by the app, which is now shared with Facebook [[50](https://www.forbes.com/sites/zakdoffman/2021/01/03/whatsapp-beaten-by-apples-new-imessage-update-for-iphone-users/?sh=6f16e1703623)].

![](https://miro.medium.com/v2/resize:fit:612/0*_Jz6cBnfh9Z4bA05.png)

**Mobile Clients**

The Android WhatsApp uses smaller than 1024 bit key to sign the app and hard codes API keys including Google Maps and Android Manifest file does not protect broadcast receivers with permissions such as Signature/SignatureorSystem that can result in data leak.

# Strengths of aforementioned Messaging Apps

Following are major strengths of the messaging apps discussed above:

## Ease of Use

WhatsApp is the gold standard for ease of use and simple messaging and other apps such as Signal, Telegram and Threema also offer similar user experience.

## Scalability

The messaging apps such as WhatsApp have been scaled to send 100B+ messages/day. This is biggest strength of tech giants that owns these platforms because they have built huge data centers for supporting this scale of messaging. On the other hand, these tech giants also use this infrastructure to extract as much data as they can from users and store them for marketing purpose or surveillance.

## Confidentiality and Data Integrity

All of the messaging apps discussed above provide end-to-end encryption and provide reasonable data integrity. Though closed source apps such as FB Messenger, WhatsApp and Telegram cannot be fully trusted because they use servers for group administration and collect a lot of metadata that hinders data privacy.

## Rapid Development

The centralized and controlled server environment allows messaging providers such as FB Messenger, WhatsApp, Signal, Telegram to rapidly develop and deploy new features. This also keeps development cycle of server side and mobile app separate as server side can be easily updated without updating mobile apps.

# Weaknesses of aforementioned Messaging Apps

Following are major drawbacks of the messaging apps discussed above:

## Single Point of Failure

The centralized API used by above messaging apps become a single point of failure and messaging apps cannot function without these APIs [[69](https://icyphox.sh/blog/signal/)].

## Bug Fixes

The centralized API adds dependency on the provider for fixing any bugs or updates and despite using open sourcing their code they may not accept issues or fix bugs reported by their users [[75](https://github.com/net4people/bbs/issues/60)].

## Walled Gardens

The popular messaging apps such as WhatsApp, FB Messenger, Signal and Threema do not use standard protocols for messaging and you cannot easily create a new client to use their services [[79](https://stuker.com/2021/whatsapp-and-most-alternatives-share-the-same-problem/)].

## Centralized Authentication

The messaging apps discussed above either use provider account for authentication or generate an account upon installation. This account is used to authenticate all requests for message routing, contacts discovery, acknowledgements, presence, notification and other operations. This design adds dependency on the central server for authentication, sending or receiving messages and you cannot use messaging when the centralized servers are not accessible [[69](https://icyphox.sh/blog/signal/)].

## Service outage

As messaging is now part of critical communication infrastructures and any disruption in availability of data centers of messaging providers affects millions of people who cannot connect with others [[68](https://www.theverge.com/2021/1/15/22232993/signal-outage-new-users-messages-not-sending)].

> _I think the_ [_@signalapp_](https://twitter.com/signalapp?ref_src=twsrc%5Etfw) _apps DDoS’ed the server. Servers ran over capacity due to influx of users and started to return HTTP 508 which was not handled by the app and millions of apps started retrying the connection at once. Judging from recent commits in_ [_https://t.co/KD6kS2o9wt_](https://t.co/KD6kS2o9wt)
> 
> _— Daniel Novak (@NovakDaniel)_ [_January 16, 2021_](https://twitter.com/NovakDaniel/status/1350471722034745348?ref_src=twsrc%5Etfw)

## Proprietary Protocols and Vendor Lock-in

The messaging apps discussed above use proprietary protocols and customized APIs to lock users into their platforms. The messaging apps with large user-base such as WhatsApp, Signal and Telegram use network effect to make it harder to interact with their system from third party clients or switch to another messaging system. These messaging platforms use network effect to prevent competition and grow larger and larger with time. This is true even for open-source messaging platforms such as Signal that does not collaborate with other open messaging specifications and prohibits use of its branding for setting up federated Signal servers or use its servers from other apps such as [LibreSignal](https://github.com/LibreSignal/LibreSignal/issues/37#issuecomment-217211165) [[52](https://drewdevault.com/2018/08/08/Signal.html), [60](https://www.oyd.org.tr/en/articles/signal/), [63](https://matrix.org/blog/2020/01/02/on-privacy-versus-freedom)].

## Phone or Email as Identity

Most of messaging apps discussed above use phone or email as identity that weakens security and privacy. The requirement of having a phone number before using end-to-end encryption burdens users who don’t have a phone number or who want to use other computing devices such as tablets for messaging. They also require sharing phone numbers with other users for messaging that infringes upon user’s privacy. The phone numbers are owned by telecommunication companies that can ban users or transfer control to malicious users via SIM swapping, social engineering or other attacks. Though, messaging apps such as WhatsApp or Signal send a warning when identity keys are changed but users rarely pay attention in practice. Both Signal and WhatsApp also supports “registration PIN lock” for account recovery that requires both phone number and registration PIN code to modify identity keys, thus reducing the efficacy of PIN locks. Most countries require identity card to obtain phone number that thwarts user privacy and can be used by governments to track whistleblowers or activists [[19](https://getsession.org/wp-content/uploads/2020/02/Session_Whitepaper.pdf), [20](https://securelist.com/large-scale-sim-swap-fraud/90353), [54](https://citizenlab.ca/2020/12/the-great-ipwn-journalists-hacked-with-suspected-nso-group-imessage-zero-click-exploit/)]. Recent Princeton study found that five major US carriers are vulnerable to SIM-swapping attacks to gain access to victim’s phone number [[32](https://www.issms2fasecure.com/assets/sim_swaps-01-10-2020.pdf), [33](https://www.engadget.com/2020-01-12-princeton-study-sim-swap.html)].

## User Data and Privacy

Big tech companies control data and privacy policies for many of these messaging apps and often integrate messaging system with other parts of their platforms such as recent changes in WhatsApp privacy policies for sharing user data with Facebook. Other messaging apps such as Signal, which recently gained a large user-base that migrated off WhatsApp lacks proper policies regarding data privacy and improper use of their services [[73](https://www.platformer.news/p/-the-battle-inside-signal)]. Due to low hardware cost, this user data is often stored indefinitely and is not only used for ad tracking but is also shared with law enforcement or government agencies without informing users.

## Metadata Collection

Though, end-to-end encryption guarantees confidentiality of message contents so that only recipient can see the contents but the message includes metadata related to sender, receiver and timestamps, which is needed for routing, delivering or ordering of the message. The messenger services can easily tap into this metadata, Geo-location via IP-addresses of requests because all messages are relayed through their servers. This data is a liability even when the messaging systems such as Signal are not actively collecting it because any malicious user or rogue employee may scrape or leak this data.

## Lack of Trustless Security

The messaging apps with centralized APIs such as WhatsApp, Signal and Telegram require trust in the company for securing data and services, which breaks basic tenant of security because truly secure systems don’t require trust [[52](https://drewdevault.com/2018/08/08/Signal.html)].

## Usage Statistics

A number of messaging apps with centralized servers collect aggregate user statistics such as number of messages, type of messages, Geo-location, etc that can be used to censor activists, journalists or human right defenders [[23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf), [54](https://citizenlab.ca/2020/12/the-great-ipwn-journalists-hacked-with-suspected-nso-group-imessage-zero-click-exploit/)].

## Message Acknowledgement

Messaging apps such as WhatsApp or Signal send acknowledgement when a message is sent or received, which is not secured by the end-to-end encryption. The results of security research showed how these messages can be intercepted or forged by someone with malicious access to the servers.

## Censorship

Messaging apps such as WhatsApp and Signal use phone number as identity that can be used to censor or ban users from using the messaging apps. As these apps use centralized APIs for relaying all messages, these servers can refuse to send or receive messages based on metadata from the API request such as sender, receiver, or Geo-location via IP-address. Due to centralized APIs, governments can easily disable access to the messaging servers and some apps including Signal support proxy servers but it’s implemented in leaky fashion that can be easily traced [[76](https://github.com/net4people/bbs/issues/63)].

## Complexity and Attack Surface

In order to excel from with other messaging apps, messaging providers have added a variety of additional features that have increased the complexity of messaging system. This complexity has become a liability for user security and privacy because it has increased the attack surface. Major security companies such as NSO Group makes millions of dollars for selling zero-day exploits that are used to decrypt messages that are supposed to be guarded by end-to-end encryption. These exploits are often used to hack, persecute or kill journalists, activists or human rights defenders [[18](https://eprint.iacr.org/2017/713.pdf), [45](https://www.wired.com/story/whatsapp-security-flaws-encryption-group-chats/), [47](https://www.dw.com/en/germanys-data-chief-tells-ministries-whatsapp-is-a-no-go/a-53474413), [48](https://medium.com/tenable-techblog/turning-signal-app-into-a-coarse-tracking-device-643eb4298447), [54](https://citizenlab.ca/2020/12/the-great-ipwn-journalists-hacked-with-suspected-nso-group-imessage-zero-click-exploit/), [66](https://bugs.chromium.org/p/project-zero/issues/detail?id=1936)].

## Closed Source or Customized implementation

Though, some of messaging apps such as Session, Signal are open source but many other apps remain close-source. An end-to-end encryption can only be trusted if the implementation is fully source-code and properly audited by an independent third party. Though, a number of messaging apps use Signal protocol for end-to-end encryption but they use a customized encryption libraries, implementation of Signal protocol and messaging protocol for authentication, communication or group management. Despite availability of open source libraries such as [libsodium](https://doc.libsodium.org/) for encryption, [olm](https://gitlab.matrix.org/matrix-org/olm)/[megolm](https://gitlab.matrix.org/matrix-org/olm/blob/master/docs/megolm.md) for double-ratchet implementation, [MLS](https://messaginglayersecurity.rocks/) standard for group chat, they have gained very little adoption in these messaging apps. As a result, there is very little interoperability among these messaging apps.

# Summary of Evaluation

Following table summarizes capabilities and characteristics of messaging apps discussed above:

# Open Messaging Standards

Following section reviews open standards for providing encrypted communication between two parties or among multiple participants in a group conversation:

# Pairwise Communication Protocols

## Email and PGP

Email uses SMTP (Simple Mail Transfer Protocol) for sending outgoing messages that was defined by IETF in 1982. Phil Zimmerman defined PGP (Pretty Good Privacy) in 1991 to support confidentiality, authenticity and end-to-end encryption, which was later became IETF standard in 1997 as OpenPGP. Email also supports S/MIME standard that uses a centralized public key infrastructure as opposed to decentralized architecture of PGP. There has been very little adoption of these standards despite their long history and have difficult to use in practice. Email envelop includes a lot of metadata such as sender, recipient, timestamps that can be used for tracking communication. Also, Email/PGP lacks adoption of modern cryptography algorithms and does not support future secrecy or post-compromise security [[23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf), [41](https://latacora.micro.blog/2020/02/19/stop-using-encrypted.html)].

## XMPP

XMPP (eXtensible Message and Presence Protocol) is a chat protocol that became an IETF standard in 2004. XMPP uses XML streams to support asynchronous, end-to-end exchange of data. There is an ongoing work to add end-to-end encryption to XMPP [[23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf)].

## Off-the-record protocol (OTR)

OTR protocol was released in 2004 as an extension to XMPP to provide end-to-end encryption, forward secrecy and deniability. OTR is designed for synchronous communication and requires both sender and receivers to be online and it does not work for group communication or when recipients are offline [[23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf)].

## Matrix

Matrix.org is an open specification for decentralized communication using JSON. It uses Olm library that implements Signal protocol for providing end-to-end encryption between two users and uses MegOlm for group chat. The Olm/MegOlm libraries use Curve25519 for generating identity keys and prekeys, Ed25519 for signing keys before upload and 3DH/Double-Ratchet algorithms for asynchronous encrypted messages [[23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf), [24](https://blog.jabberhead.tk/2019/03/10/a-look-at-matrix-orgs-olm-megolm-encryption-protocol/)]. Matrix also provides interoperability with other protocols and can be used as a bridge to support other messaging apps [[36](https://matrix.org/docs/spec/), [65](https://matrix.org/docs/guides/whatsapp-bridging-mautrix-whatsapp)].

## Open Whisper System Signal Protocol

The Signal protocol was created by Moxie Marlinspike for Open Whisper System and TextSecure messaging app to provide end-to-end encryption so that only intended recipient can see the plaintext message. It was later adopted by Signal app, WhatsApp, Google Allo (defunct), FB Messenger and other messaging apps. The Signal Protocol uses a rachet system that changes the encryption key after each message for providing forward secrecy or post-compromise security.

This protocol requires each user to generate a long-term identity key-pair, a medium-term signed prekey pair and several ephemeral keys using Curve25519 algorithm. The public keys of these pairs are packed into prekey bundle and uploaded to a Key Exchange Server/Key Distribution Center for dissemination. The prekey bundle is downloaded by other users before sending a message, which is then used to build a new session key based on message sender and recipient keys using Extended Triple Diffie-Hellman (X3DH) key agreement protocol. This protocol can work with offline users where a third party server can store and forward the messages when the user is back online.

The Signal protocol uses AES-256 for encryption and HMAC-SHA256 for validating data integrity. The X3DH generates a shared secret key from X3DH, which is used to derive “root key” and “sending chain key”. These keys are used by Double Ratchet (DR) Algorithm, which is derived from [Off-the-Record protocol](https://otr.cypherpunks.ca/Protocol-v3-4.1.1.html). The DR algorithm derives a unique message key using a key derivation function (KDF) that is then used to send and receive encrypted messages. The outputs of the DH ratchet at each stage with some additional information to advance receiving key chain are attached to each encrypted message. The “sending chain key” is advanced when a message is sent and sender derives a new “sending chain key”. Similarly, receiver advances “receiving chain key” to generate new decryption key. The “root key” is advanced upon initialization session so that each message uses new ephemeral or ratchet key that guarantees forward-secrecy and post-compromise security. The first message also attaches sender’s prekey bundle so that receiver derive the complementary session [[12](https://www.signal.org/docs/specifications/doubleratchet/), [13](https://www.signal.org/docs/specifications/x3dh/)].

## OMEMO

OMEMO is an extension of XMPP develped in 2015 to provide end-to-end encryptio using Signal protocol [[23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf)].

## The Messaging Layer Security (MLS) Protocol

[Messaging Layer Security](https://messaginglayersecurity.rocks/) (MLS) is a standard protocol being built by IETF working group for providing end to end encryption. It defines specification for security context in [architecture document](https://messaginglayersecurity.rocks/mls-architecture/draft-ietf-mls-architecture.html) and specification for protocol itself in [protocol document](https://messaginglayersecurity.rocks/mls-protocol/draft-ietf-mls-protocol.html). Each user publishes init keys consisting of credentials and public keys ahead of time that are handled by delivery service. There are two types of messages: handshake messages, which are control messages for group membership with global order and application messages with text/multimedia contents.

# Group Messaging Protocols

Group messaging with 3+ members require additional complexity with end-to-end encryption. Each messaging app uses its own protocol for group management and communication among members. Following are some solutions that are used in messaging apps:

## mpOTR

The multi-party off-the-shelf messaging (mpOTR) [3] extends OTR for providing security and deniability. It uses a number of interactive rounds of communication where all parties have to be online, which doesn’t work in presence of unreliable or mobile networks [[2](https://eprint.iacr.org/2017/666.pdf)].

## Sender-keys with Signal Protocol

Sender-keys was developed by Signal protocol and is used by a number of messaging apps such as libSignal, Whatsapp, Google Allo (defunt), FB Messenger, Session and Keybase for group messaging but it’s no longer used by Signal app. A user generates the sender-key randomly, encrypts it for each user using the pair key and then sends it to that user. As this sender key is reused for group communication and is not refreshed or removed after each message, this protocol cannot guarantee forward-secrecy or PCS. This protocol allows messenger to send a message once to their delivery server that then fans out the message to each member of the group. These messaging apps regenerate sender-key when a membership is updated, which creates O(N²) messages for a group of size N because each member has to create a new sender key and send it to other group members. However, a bad actor in the group can eavesdrop message communication until that member is removed and sender keys are refreshed [[2](https://eprint.iacr.org/2017/666.pdf), [5](https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/), [18](https://eprint.iacr.org/2017/713.pdf)].

## Open Whisper System Signal Protocol

As Signal protocol uses sender-keys for group communication, it requires encrypting and sending message for each group member, which takes O(N²) messages for a group with size N. Also, the double ratchet algorithm is very expensive in group chat because it requires each message has to generate new session keys for each other group member. Group messaging also adds more complexity to authentication, participant consistency for group membership and ordering of messages.

The Double-Rachet does not preserve forward-secrecy or post-compromise-security (PCS) in group messaging due to high computation and bandwidth overhead and Signal messenger instead uses sender-keys in group-communication that can compromise entire group without informing the users even if a single member is compromised. Regularly, rebroadcasting sender-keys over secure pairwise channel can prevent this attack but it doesn’t scale as group size increases [[1](https://research.fb.com/wp-content/uploads/2018/11/On-Ends-to-Ends-Encryption-Asynchronous-Group-Messaging-with-Strong-Security-Guarantees.pdf#cite.0@cohn2016post), [2](https://research.fb.com/wp-content/uploads/2018/11/On-Ends-to-Ends-Encryption-Asynchronous-Group-Messaging-with-Strong-Security-Guarantees.pdf)].

When sending a message to group, the Signal app sends a message for each message to their delivery API that then stores and forward the message to the recipients.

## Asynchronous Ratcheting Trees (ART)

Asynchronous Ratcheting Trees (ART) offers confidentiality, authenticity, strong security (PCS) with better scalability as group size increases. The ART protocol trusts initiator and in order to support asynchronicity, it supports weaker security if members are offline similar to zero round-trip mode of TLS 1.3. It uses signature to authenticate initial group message and a MAC to authenticate subsequent updates. A tree-based group DH protocol is used to manage group members and a member updates personal keys in this tree structure to guarantee PCS [[2](https://research.fb.com/wp-content/uploads/2018/11/On-Ends-to-Ends-Encryption-Asynchronous-Group-Messaging-with-Strong-Security-Guarantees.pdf)].

## The Messaging Layer Security (MLS) Protocol for Groups

The MLS protocol is an IETF standard for providing asynchronous end-to-end encrypted messaging. The [MLS protocol](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/?include_text=1) provides more efficient key change for providing end-to-end encryption with forward security and PCS for group messaging where group size can scale up to thousands of members [[4](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol)]. The MLS protocol uses a rachet tree, which is a left-balanced binary tree whose leaves represent a member and intermediate nodes carry a Diffie-Hellman public key and private key [[5](https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/)].

![](https://miro.medium.com/v2/resize:fit:700/0*maz0WtewOLVCuY4A.png)

Each member retains a copy of the rachet tree with public keys but private keys are shared using the rachet-tree property:

> _If a member M is a descendant of intermediate node N, then M knows the private key of N._

The root is labeled with shared symmetric key known to all users. The _s_ender keys are derived using key-derivation function (KDF) from the root node’s key and the private keys are distributed to copath nodes when a member is removed [[2](https://research.fb.com/wp-content/uploads/2018/11/On-Ends-to-Ends-Encryption-Asynchronous-Group-Messaging-with-Strong-Security-Guarantees.pdf)]. Unlike signal-protocol that results in O(N²) messages after a membership change, MLS only costs O(N) for initially adding users and O(N log N) thereafter when keys are refreshed [[7](https://mattweidner.com/acs-dissertation.pdf)].

Each group in MLS is identified by a unique ID and it keeps a epoch number or a version to track changes to the membership. The epoch number is incremented upon each change to the membership, e.g.

![](https://miro.medium.com/v2/resize:fit:700/0*KEdt26MH5vOh3Obd.png)

The MLS protocol guarantees membership consistency by including group membership in shared key derivation during key negotiations. For example, a group operation includes group-identifier, epoch number that represents version and content-type to determine the ordering requirements [[4](https://architecture.messaginglayersecurity.rocks/)].

## Casual TreeKEM

Causal TreeKEM [[7](https://mattweidner.com/acs-dissertation.pdf)] uses CRDT (Conflict-free Replicated Data Type) to improve TreeKEM by eliminating the need for a linear order on changes to group membership. The CRDTs allow replication of mutable data among group of users with the guarantee that all users see the same state regardless of the order in which updates are received. This allows building collaborative applications with end-to-end encryption. The CRDTs requires casual order to support partial order for each sender but it doesn’t require linear order. However, users cannot delete old until they have received all concurrent state change messages, which diminishes guarantees of forward secrecy.

# Essential Features and Design Goals of a New Messaging System

Based on evaluation of current landscape of messaging apps and lack of adoption of open standards and decentralized architecture, this section recommends essential features and design goals for a new messaging system that can address these limitations and provide better support for decentralization, trustless security, scalability of group conversation, minimization of data collection, censorship, etc. Following sections delineate these features and design goals:

# Open Protocols and Standards

Internet was designed by ARPA in 1960s to replace centralized network switches with a network of nodes. Internet defined open protocols such as TCP/UDP/IP for network communication, RIP/BGP for routing, DNS for domain name lookup, TELNET/FTP for remote access, POP3/IMAP/SMTP for emails, IRC for chat, NNTP for usenet groups. Sir Tim Berner-Lee used similar design principles for world-wide-web when he designed HTTP protocols. These protocols were designed with decentralized architecture to withstand partial failure in case of attacks by atomic bomb. For example, DNS architecture is designed as a hierarchical tree where root level node manages DNS nodes for top-level-domains (TLD), secondary authoritative TLD DNS node maintains list of authoritative DNS nodes for each domain and lower level nodes maintains subdomains.

![](https://miro.medium.com/v2/resize:fit:700/0*bRYt9INDiscxTyKj.png)

Similarly, SMTP for email delivery uses MX record in DNS to find Message transfer agent (MTA) server and delivers the message. In addition, SMTP uses a relay protocol when target MTA is not on the same network as sender to forward the message to another MTA that keeps forwarding the message until it reaches the target MTA.

![](https://miro.medium.com/v2/resize:fit:700/0*482qm6TpIUlhUPTq.png)

The proposed messaging system will instead use open standards and protocols similar to Email/SMTP and HTTP(s) as advocated by [Protocols, Not Platforms: A Technological Approach to Free Speech](https://knightcolumbia.org/content/protocols-not-platforms-a-technological-approach-to-free-speech). Also, in absence of open standards, de-facto industry standards such as Signal protocol will be used.

# Decentralized Architecture

Previous section discussed deficiencies of centralized messaging systems such as FB Messenger, WhatsApp, Signal, Telegram, etc. The proposed messaging system will use decentralized or federated servers similar to Email/SMTP design that can relay messages from one server to another or deliver messages to recipients.

![](https://miro.medium.com/v2/resize:fit:700/1*9oAbKWCCAIUUwtoYjncBjw.png)

The decentralized architecture may need peer-to-peer communication layer such as [Tor](https://www.torproject.org/) or [I2P](https://geti2p.net/en/) used by [Briar](https://briarproject.org/), [libp2p](https://libp2p.io/) used by [Status.im](https://status.im/) app or a network of nodes and onion routing used by [Session](https://getsession.org/session-protocol-technical-information/) app.

# Open Source

The proposed messaging system will use open source libraries such as [libsodium](https://doc.libsodium.org/) for encryption, [olm](https://gitlab.matrix.org/matrix-org/olm)/[megolm](https://gitlab.matrix.org/matrix-org/olm/blob/master/docs/megolm.md) for double-ratchet implementation, [MLS](https://messaginglayersecurity.rocks/) standard for group chat and [matrix](https://matrix.org/docs/spec/) for bridging with existing messaging systems.

# Asynchronicity

Asynchronicity allows users to send messages, share keys or manage group members when other users are offline. Asynchronicity requires that we cannot use standard DH key exchange used in TLS protocol and instead pre-computed prekeys of recipients are used to encrypt the messages [[2](https://eprint.iacr.org/2017/666.pdf)]. The messaging system will allow sending messages to offline users by using a decentralized storage that can store encrypted messages.

# Secrecy and Confidentiality

The proposed messaging system will guarantee secrecy and confidentiality so that only recipient user can compute the plaintext of the message when communicating in one-to-one conversation or in a group conversation.

# Authenticity with Decentralized Identity

The proposed messaging system will use decentralized identity and will allow participants to validate identity of other members of conversations.

# Authentication, Deniability and Non-repudiation

Deniability or deniable authentication allows conversation participants to authenticate the origin of the message but prevents either party from proving the origin of the message to a third party post-conversation. Non-repudiation is opposite of deniability and it is a legal term that provides proof of the origin of data and integrity of the data. Centralized messengers such as FB Messenger uses Authentication to prevent impersonation or deniability but public/private keypair based authentication supports deniability and it is considered a feature in protocols such as OTR, mOTR and WhatsApp. One of the ways to provide deniability is to use ephemeral signature keys when messages are signed by medium-term signature keys [[7](https://mattweidner.com/acs-dissertation.pdf)].

# Data Integrity

The data integrity guarantees that data is not tempered or modified during transmission and the messaging system will use Message Authentication Code (MAC) to validate message integrity and detect any modification to the message.

# Forward Secrecy and Post-Compromise Security

Forward secrecy protects former messages from being read when long-term key information is exposed. Similarly, Post-compromise security guarantees that if adversary gets hold of keys, they cannot be used to decrypt messages indefinitely after the compromise is healed. The proposed messaging system will use [Signal](https://signal.org/docs/) and [Message Layer Security (MLS)](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) protocols to guarantee forward secrecy and post-compromise security. These protocols use Diffie-Hellman Exchange to establish an pre-computed/ephemeral DH prekeys for each conversation. The forward secrecy for each message is achieved by additionally using deterministic key ratcheting that derives shared key using a chain of hash functions and deletes keys after the use [10].

# Minimize Metadata Collection

The decentralized architecture will protect from collecting metadata from centralized servers but proposed messaging system will further reduce any exposure to metadata by using design principles from [Session](https://getsession.org/) app such as using self-hosted infrastructure for storing attachments, using onion routing to hide IP-addresses of users in transport message-exchange layer and using public/private keypair for identity as opposed to phone number or email address [[19](https://getsession.org/wp-content/uploads/2020/02/Session_Whitepaper.pdf)].

# Network Anonymity

The proposed messaging system may use P2P standards such as [Tor](https://www.torproject.org/) or [I2P](https://geti2p.net/en/) to protect metadata or routing data. For example, Pond, Ricochet, Briar provides network anonymity by using Tor network but they don’t hide metadata such as sender, receiver, timestamp, etc. The Session app protects metadata such as IP-addresses of senders and receivers using onion network [[19](https://getsession.org/wp-content/uploads/2020/02/Session_Whitepaper.pdf), [23](https://hal.inria.fr/hal-01426845/file/paper_21.pdf)].

# Message Acknowledgement

The proposed messaging system will use end-to-end encrypted messages for acknowledgement so that they cannot be intercepted or forged by someone with access to centralized server [[18](https://eprint.iacr.org/2017/713.pdf)].

# Turn off Backup

In order to guarantee robust data privacy, the messaging app will turn off any backup of chat history offered by IOS or Android APIs.

# Access Control for Group Administration

A member in a group will be able to perform following actions:

- Create a group and invite others to join
- Update personal keys
- Request to join an existing group
- Leave an existing group
- Send a message to members of the group
- Receive a message from another member in the group

The creator of group will automatically become admin of the group but additional admins may be added who can perform other actions such as:

- Add member(s) to join an existing group
- Remove member(s) from an existing group
- Kick a member from an existing group

A group will define set of administrators who can update the membership and will be signed by the administrator’s group signature key. Further, a group secret or ticket will be used for proof of membership so that each member verifies the ticket and guest list of group [[18](https://eprint.iacr.org/2017/713.pdf)].

# Group Communication

The proposed messaging system will use [Message Layer Security (MLS)](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) protocol and Tree-KEM based ratchet trees to manage group keys in scalable fashion and to support large groups. This will provide better support forward-secrecy in a group conversation where keys can be refreshed with O(N Log N) messages as opposed to O(N²) messages in Signal protocol.

# Multiple Devices

A user can use multiple devices where each device is considered a new client. Though, a user can add new device but that device won’t be able to access message history. The multiple devices by same user can be represented by a sub-tree under the user-node in [Tree-KEM](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/). User can publish the public key of their sub-tree as an ephemeral prekey and hide actual identity of the device so that you cannot reveal the device that triggered the update [[2](https://eprint.iacr.org/2017/666.pdf)].

# Account Recovery

The messaging app will use 12–24 words mnemonic seed to generate long-term identity keypair similar to [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) standard to generate deterministic wallets for [Bitcoin](https://github.com/bitcoin/bips/tree/master/bip-0039). Alternatively, the messaging system may implement social recovery similar to [Argent wallet](https://www.argent.xyz/) and the [Loopring wallet](https://loopring.io/) [[61](https://vitalik.ca/general/2021/01/11/recovery.html)].

# Monetary Incentive

Decentralized architecture requires new infrastructure for peer-to-peer communication, decentralized identity, keys management, etc. Public peer-to-peer infrastructures are not designed to handle the needs of modern messaging throughput of 100B+ messages/day. In order to scale this infrastructure, the node-runners will require a monetary incentive to expand this infrastructure. For example, a number of blockchain based projects such as [Sia](https://sia.tech/), [Storj](https://storj.io/), and [Serto](https://www.serto.id/) use crypto rewards for providing storage or network services.

# Disappearing Messages

This feature will be implemented by the mobile messaging app to automatically delete old messages in order to bolster the security in case the phone is seized or stolen.

# Contacts Discovery

As contacts on the phone are generally searchable by phone number or email so they can be readily used for connecting with others on the messaging system that use Phone number or email for identity. Decentralized messaging systems that use a local a public/private keypair for identity such as [Status](https://status.im/) and [Session](https://getsession.org/) cannot use these features. Instead, they rely on a different lookup service, e.g. Session uses [Loki Name Service](https://loki.network/2020/03/25/loki-name-system-the-facts/) and Status uses [Ethereum Name Service](https://our.status.im/usernames-in-the-status-messenger/) to register usernames that can be used to connect with friends.

# Attachments/Multimedia Messages

Small attachments and multi-media files can be embedded with the messages using the same end-to-end encryption but large attachments and multimedia files can be shared via a decentralized storage system. The large files can be uploaded with symmetric encryption that will be available for a limited time and symmetric password can be shared via exchange of encrypted messages.

# Compromise of User Devices

Due to its decentralized nature, this messaging system does not suffer any security risk if decentralized authentication is compromised. All user data and keys will be stored locally and if an attacker gets hold of user or group’s private keys, he may be able to send messages impersonating as the user. However, the attacker won’t be able to access future messages if user or group’s keys are refreshed and old keys are deleted from the device.

# Data Privacy Regulations

This messaging system will provide better support for the data privacy regulations such as General Data Protection Regulations (GDPR) from the EU or the California Consumer Privacy Act that allow users to control their data and privacy.

# Offline Members

As forward-secrecy and post-compromise-security rely on updating member keys, an offline member will be holding old keys and thus may be susceptible to compromise.

# Proposed Messaging Architecture

The architecture for building a new messaging system is largely inspired by design philosophy of Internet that was designed as decentralized to withstand nuclear attack and by vision of Sir Tim Berners-Lee for using open protocols such as SMTP, FTP, HTTP(S). The data privacy architecture is influenced by Web 3.0 architecture for building decentralized applications (DApps) that keep user data private and securely communicate with other systems using encrypted channels in a trustless fashion. As the users use increasingly powerful devices with high-bandwidth network, this architecture leverages edge computing to bring computation closer to the user device where private data is securely stored as opposed to storing this data in cloud services. However, this does not preclude storing user-data remotely as long as it’s encrypted and the owner of data can control who can access it based on privacy preferences.

Following building blocks are defined for designing this messaging system based on open standards and decentralized architecture.

![](https://miro.medium.com/v2/resize:fit:700/0*d0v7PqALRh7ECkqY.png)

# Decentralized Services

The proposed messaging architecture uses following decentralized services:

## Decentralized Identity

The [Decentralized Identity Foundation](https://identity.foundation/) (DIF) and the [W3C Credentials Community Group](https://www.w3.org/community/credentials/) are standard bodies that are building open standards for decentralized identity such as [Decentralized identifiers](https://w3c.github.io/did-core/) (DID) that use public/private keypair for generating user identities. The users hold their identities in their digital wallet that can be used to control access to personal data or identity hubs, form relationships with other entities and communicate with other users. The user identity contain a decentralized identifier (DID) that resemble Uniform Resource Identifier (URI) and specifies location of service that hosts DID document, which includes additional information for identity verification [[8](https://w3c.github.io/did-core/)]. A user manages access control via private keys accessible only to the user. A user may also create multiple identities for different service providers using multiple DID based identifiers and user consent is required to access claims for providing granular access control [[9](https://www.microsoft.com/en-us/security/business/identity/own-your-identity)].

Though, using this standard is not a strict requirement for this architecture but it will be beneficial to describe user identity in a self-describing format so that dependent services can handle identity consistently.

## Decentralized Key Exchange Servers

The key-exchange servers are needed to store and disseminate public keys or prekey-bundles for each user based on their identities. Each user uploads their public keys/prekey bundles when they join and updates those prekey bundles periodically when new ephemeral keys are generated. However, such a service can be breached or a malicious directory service may impersonate a user so a public log such as [Key Transparency](https://keytransparency.org/) may be needed that publishes the binding between identity and keys in a public log.

## Decentralized Storage

Asynchronicity feature requires that users are able to send messages, share keys or manage group members when other users are offline. The decentralized storage is used as a temporary queue to store encrypted messages that are yet to be delivered. Also, users may share large attachments or media files such as pictures/videos/audios, which can be stored on the decentralized storage and then a link to the attachments are shared with other users via secure messaging. This will be further protected by symmetric encryption and decentralized identity claims to provide safe access to the shared file. A number of decentralized storage services such as [IPFS](https://ipfs.io/), [Storj](https://storj.io/), [Sia](https://sia.tech/), [Skynet](https://siasky.net/), etc are already available that can be used to build decentralized messaging.

## Federation and Peer to Peer Communication

A decentralized system may need a peer to peer communication such as [libp2p](https://libp2p.io/) or [i2p](https://geti2p.net/en/) for communication. Alternatively, it may just use a federated servers similar to Email/SMTP that can route a messages from one server to another or deliver the message to a local user.

# Mobile App / Desktop Client

The mobile app or desktop client will use following components to provide secured messaging:

## Encryption and MLS Libraries

The mobile app will use open source encryption libraries such as as [libsodium](https://doc.libsodium.org/) for encryption, [olm](https://gitlab.matrix.org/matrix-org/olm)/[megolm](https://gitlab.matrix.org/matrix-org/olm/blob/master/docs/megolm.md) for double-ratchet implementation and [MLS](https://messaginglayersecurity.rocks/) for group communication.

## Digital Wallet for Keys

The digital wallet behaves as a registry of keys, which will be created after app installation and it will securely store key-pairs and key-bundles of user, contacts and groups. This wallet will be encrypted using user’s master password and a local device password, which is automatically generated. The wallet is updated when a user updates the ephemeral keys or other members update their key-bundles. In order to satisfy post compromise security, it removes old ephemeral keys and keys of deleted group member. The messaging app will use open standards such as [BIP39](https://github.com/bitcoin/bips/tree/master/bip-0039)/[BIP43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki) to create deterministic identity using mnemonic seed of phrases and [BIP32](https://github.com/bitcoin/bips/tree/master/bip-0032)/[BIP44](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki) for hierarchical deterministic format for storing keys so that they can be easily exported to another device by the owner. Another benefit of using these standards is that this hierarchy allows storing crypto-keys for crypto-assets and using them for payments.

## Group Management

This component provides group administration such as

- Create a group and invite others to join
- Join an existing group
- Leave an existing group
- Add member(s) to join an existing group
- Remove member(s) from an existing group

## Status / Presence

It’s hard to sync contacts with status and presence in a decentralized architecture because no single server maintains all contacts. However, this architecture may use a Gossip Protocol that periodically exchanges presence with a subset of contacts along with other list of contacts who are known to be online.

## Local Push Notification

The push notification is also hard to implement in a decentralized architecture and decentralized messaging apps such as [Session](https://getsession.org/) supports push notification in foreground mode. Alternatively, the mobile app can wake up periodically to poll for messages in background and use a local notification to notify users when new messages arrive. However, periodic waking up the app may drain phone battery if it’s run too often.

## Local Authentication

A unique device password will be automatically generated upon installation and the user will then create a master password, which will be used for local authentication. The device and master password will be used to encrypt identity, signature and ephemeral keypairs in the digital wallet. The user will be required to authenticate locally before using the app to decrypt these keys, which in turn will be used to decrypt outgoing or incoming messages.

## Local Delivery Service for Message Broadcast

The delivery service in this decentralized architecture will run locally on the client side to send outgoing messages. It will use the transport layer to send messages asynchronously that may use protocol-bridge to integrate with other messaging systems or use decentralized storage to store messages when recipients are offline. The delivery service interacts with local digital wallet to encrypt outgoing messages using [Signal](https://www.signal.org/docs/)/[MLS](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) protocols. The delivery service routes both user-messages and meta-messages that include user-invitation, changes to group membership, etc. The local delivery service also receives incoming messages, decrypt them using double-ratchet algorithm and user’s private keys in the key registry. The incoming messages are then stored in local message store for direct access from the messaging app.

## Transport Message-Exchange

The transport message-exchange is a network layer for sending or receiving messages. This component only handles encrypted outgoing and incoming messages without access to private keys. If message recipients are not online, it will use decentralized storage to store and forward messages. It may use protocol bridge to support other protocols such as email/SMTP, XMPP, Matrix, Gossip Protocol, P2P, etc. to synchronize message datasets directly [13, 14].

## Protocol Bridge

This component will provide interoperability with other messaging protocols and may use [Matrix](https://matrix.org/) protocol to bridge with other messaging apps [[36](https://matrix.org/docs/spec/), [65](https://matrix.org/docs/guides/whatsapp-bridging-mautrix-whatsapp)].

## Local Message Store

The local message-store stores all outgoing and incoming messages that can be viewed by user in messaging app. The local message store keeps messages secured using symmetric encryption using user’s master password and device specific password. The messages will only be displayed after local authentication. The local message-store may subscribe to local delivery service to display new messages when they are received. The user may also specify retention policy for messages to archive or remove old messages.

## Local Directory of Contacts

The local directory of contacts keeps metadata about contacts and groups such as name, avatar, etc.

## Messaging Adapter

The messaging adapter abstracts core messaging functionality and provides high level APIs that UI can use to manage contacts, groups or send/receive messages.

## Self-Destruct Button

In order to protect user’s private data when their phone is stolen or seized, the messaging app may allow users to delete all data that can be triggered remotely using a special poison message, self-destruct timer or poison PIN [[39](https://techcrunch.com/2019/06/12/every-secure-messaging-app-needs-a-self-destruct-button/)].

# High-level Applications

Decentralized messaging with end-to-end encryption is an indispensable component for building decentralized applications. A message can be used to represent data, an event or a command that can be communicated with end-to-end encryption and validate integrity of messages using digital signatures based on public/private keypair. Following section describes a few examples of such decentralized applications that can be built on top of secure messaging:

# Social platforms

The social platform such as Facebook, YouTube, Twitter are facing increasingly demands to address wide spread hatred, fake news and conspiracy theories on their platform. The Communications Decency Act, Section 230 protects these platforms from being liable due to user contents but tech companies face increasing pressure to police their contents. These problems were highlighted by Mike Masnick in his article [Protocols, Not Platforms: A Technological Approach to Free Speech [6]](https://knightcolumbia.org/content/protocols-not-platforms-a-technological-approach-to-free-speech). He advocated building protocols like Internet protocols such as SMTP, IRC, NNTP and HTTP as opposed to platforms controlled by tech giants. It would delegate responsibility of tolerating speech to end users and controlling contents to the end of network. For example, [ActivityPub](https://www.w3.org/TR/activitypub/) and [OpenSocial](https://github.com/OpenSocial/spec/) are open specification for social network platforms and a number of decentralized social platforms are already growing such as [Scuttlebutt](https://handbook.scuttlebutt.nz/), [Gurlic](https://gurlic.com/), [Mastodon](https://joinmastodon.org/), [PeerTube](https://joinpeertube.org/), [Serto](https://www.serto.id/), [Fediverse](https://fediverse.party/), [Pixelfed](https://pixelfed.org/), [WriteFreely](https://writefreely.org/), [Plume](https://joinplu.me/), [Funkwhale](https://funkwhale.audio/), etc.

# Source Code Management

The source code management systems such as Git or Mercurial were designed as distributed and decentralized architecture but commercial offerings such as Github, Atlassian, Gitlab hijacked them by packaging it with additional features such as code-reviews, release management, continuous integration, etc. However, as with any central control systems such services are subject to censorship, e.g.

> [@GitHubHelp](https://twitter.com/GitHubHelp?ref_src=twsrc%5Etfw) , you blocked our entire company account after one employee opened his laptop while visiting is parents in Iran. We are completely blocked from deploying! (@sebslomski) [December 30, 2020](https://twitter.com/sebslomski/status/1344219609923276801?ref_src=twsrc%5Etfw)

The decentralized identity, key management system and storage systems discussed in this paper can help revive original vision of these systems. As decentralized systems require smart clients or edge computing, many of additional features such as code-reviews can be built directly into the clients or other decentralized applications. The messaging protocol can be used to trigger an event for integration with other systems, e.g. when a source code is checked, it fires a message to the build system that kicks off build/integration, which in turn can send message(s) to developer group or other systems upon completion or failure.

# Audio and Video Communication

A messaging app will be incomplete without audio and video communication but these features can be built on top of messaging protocols with end-to-end encryption.

# Collaborative Tools

You can build communication / collaborative tools such as Google docs using a layer of Conflict-free Replicated Data Types (CRDTs) on top of end-to-end encryption [[7](https://mattweidner.com/acs-dissertation.pdf)].

# Document Signing

The document signing features such as offered by DocuSign can be implemented using digital identities and signatures primitives used in secure messaging to sign documents safely and share these documents with other stake holders.

# IoT Software/Firmware Upgrades

Despite widespread adoption of IoT devices, their software/firmware rarely use strong encryption for upgrades and can be susceptible to hacking. Instead, end-to-end encryption, signed messages by vendor and attachments can be used to securely upgrade software.

# Health Privacy

The health privacy laws such as HIPAA can benefit from strong end-to-end encryption so that patients and doctors can safely communicate and share medical records.

# Edge Computing

Modern consumer and IoT devices such as smart phones, tablets provide powerful processing and network capabilities that can be used to move computation closer to the devices where user data is stored safely. Edge computing will improve interaction with applications because data is available locally and will provide stronger security.

# Conclusion

Secure and confidential messaging is an indispensable necessity to communicate with your family, friends and work colleagues safely. However, most popular messaging apps use centralized architecture to control access to their services. These messaging apps lack trustless security and truly secure systems don’t require trust. The tech companies running these services use network effect as a moat to prevent competition because people want to use the messaging platform that is used by their friends, thus these platforms get gigantic with time. Further, a number of messaging apps collect metadata that is integrated with other products for tracking users, marketing purpose or surveillance. This paper reviews open standards and open source libraries that can be used to build a decentralized messaging system that provides better support for data privacy, censorship, anonymity, deniability and confidentiality. It advocates [using protocols rather than building platforms](https://knightcolumbia.org/content/protocols-not-platforms-a-technological-approach-to-free-speech). Unfortunately, protocols are difficult to monetize and slow to adopt new changes so most messaging apps use centralized servers, proprietary APIs and security protocols. Using open standards and libraries will minimize security issues due to bad implementation or customized security protocols. It also helps interoperability so that users can choose any messaging app and securely communicate with their friends. Decentralized architecture prevents reliance on a single company that may cut off your access or disrupt the service due to [outage](https://techcrunch.com/2020/11/25/amazon-web-services-outage-takes-a-portion-of-the-internet-down-with-it/). The open standards help build other decentralized applications such as communication, collaboration and payments on top of messaging. This architecture makes use of powerful devices in hands of most people to store private data instead of storing it in the cloud. Internet was built with decentralized protocols such as Email/SMTP and DNS that allows you to setup a federated server with your domain to process your requests or relay/delegate requests to other federated servers. The messaging apps such as [Briar](https://briarproject.org/), [Matrix](https://matrix.org/docs/spec/), [Session](https://getsession.org/) and [Status](https://status.im/) already support decentralized architecture, however they have gained a little adaption due to lack of features such as VoIP and difficulty of use. This is why open standards and interoperability is critical because new messaging apps can be developed with better user experience and features. This will be challenging as decentralized systems are harder than centralized ones and security protocols or messaging standards such as [Signal protocol](https://www.signal.org/docs/) or [MLS](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) are not specifically designed with decentralized architecture in mind. But the benefits of decentralized architecture outweigh these hardships. Just as you can choose your email provider whether it be GMail, Outlook, or other ISPs and choose any email client to read/write emails, open standards and decentralization empowers users to choose any messaging provider or client. In the end, decentralized messaging systems with end-to-end encryption provide trustless security, confidentiality, better protection of data privacy and a freedom to switch messaging providers or clients when trust of the service is broken such as recent changes to WhatsApp’s [privacy policies](https://www.macrumors.com/2021/01/06/whatsapp-privacy-policy-data-sharing-facebook/).

# References

1. Katriel Cohn-Gordon, Cas Cremers, and Luke Garratt. 2016. On post-compromise security. In Computer Security Foundations Symposium (CSF), 2016 IEEE 29th.IEEE, 164–178.
2. Katriel Cohn-Gordon, Cas Cremers, and Luke Garratt. [On Ends-to-Ends Encryption Asynchronous Group Messaging with Strong Security Guarantees](https://research.fb.com/wp-content/uploads/2018/11/On-Ends-to-Ends-Encryption-Asynchronous-Group-Messaging-with-Strong-Security-Guarantees.pdf).
3. Ian Goldberg, Berkant Ustaoglu, Matthew Van Gundy, and Hao Chen. 2009. Multi-party off-the-record messaging. In ACM CCS 09. Ehab Al-Shaer, SomeshJha, and Angelos D. Keromytis, (Eds.) ACM Press, (Nov. 2009), 358–368.
4. The Messaging Layer Security (MLS) Protocol. [https://datatracker.ietf.org/doc/draft-ietf-mls-protocol](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol).
5. Better Encrypted Group Chat. [https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/](https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/).
6. Protocols, Not Platforms: A Technological Approach to Free Speech, August 21, 2019. [https://knightcolumbia.org/content/protocols-not-platforms-a-technological-approach-to-free-speech](https://knightcolumbia.org/content/protocols-not-platforms-a-technological-approach-to-free-speech).
7. Matthew A. Weidner, Churchill College. Group Messaging for Secure Asynchronous Collaboration (dissertation), June 6, 2019. [https://mattweidner.com/acs-dissertation.pdf](https://mattweidner.com/acs-dissertation.pdf).
8. Decentralized Identifiers (DIDs) v1.0. [https://w3c.github.io/did-core/](https://w3c.github.io/did-core/).
9. Microsoft Decentralized identity. [https://www.microsoft.com/en-us/security/business/identity/own-your-identity](https://www.microsoft.com/en-us/security/business/identity/own-your-identity)
10. Vinnie Moscaritolo, Gary Belvin, and Phil Zimmermann. Silent circle instant messaging protocol protocol specification. Technical report, 2012.
11. The Double Ratchet Algorithm [https://www.signal.org/docs/specifications/doubleratchet/](https://www.signal.org/docs/specifications/doubleratchet/).
12. The X3DH Key Agreement Protocol [https://www.signal.org/docs/specifications/x3dh/](https://www.signal.org/docs/specifications/x3dh/).
13. Prince Mahajan, Srinath Setty, Sangmin Lee, Allen Clement, Lorenzo Alvisi, MikeDahlin, and Michael Walfish. Depot: Cloud storage with minimal trust.ACM Trans.Comput. Syst., 29(4):12:1–12:38, December 2011.
14. Joel Reardon, Alan Kligman, Brian Agala, Ian Goldberg, and David R. Cheriton.KleeQ : Asynchronous key management for dynamic ad-hoc networks. Tech report, 2007.
15. WhatsApp. WhatsApp encryption overview, 2017. [https://www.whatsapp.com/security/WhatsApp-Security-Whitepaper.pdf](https://www.whatsapp.com/security/WhatsApp-Security-Whitepaper.pdf).
16. Leaking Data on Intel CPUs via Cache Evictions. [https://arxiv.org/pdf/2006.13353.pdf](https://arxiv.org/pdf/2006.13353.pdf).
17. How SGX Fails in Practice. [https://sgaxe.com/files/SGAxe.pdf](https://sgaxe.com/files/SGAxe.pdf).
18. Paul Rosler, Christian Mainka, and Jorg Schwenk. More is less: On the end-to-endsecurity of group chats in Signal, WhatsApp, and Threema. In2018 IEEE EuropeanSymposium on Security and Privacy (EuroSP), pages 415–429, April 2018. [https://eprint.iacr.org/2017/713.pdf](https://eprint.iacr.org/2017/713.pdf).
19. Kee Jefferysm, Maxim Shishmarev, Simon Harman. Session: A Model for End-To-End Encrypted Conversations With Minimal Metadata Leakage. February 11, 2020. [https://getsession.org/wp-content/uploads/2020/02/Session_Whitepaper.pdf](https://getsession.org/wp-content/uploads/2020/02/Session_Whitepaper.pdf).
20. Cryptonote 2.0. [https://cryptonote.org/whitepaper.pdf](https://cryptonote.org/whitepaper.pdf).
21. Large-scale SIM swap fraud. [https://securelist.com/large-scale-sim-swap-fraud/90353/](https://securelist.com/large-scale-sim-swap-fraud/90353/).
22. WhatsApp updates its Terms and Privacy Policy to mandate data-sharing with Facebook. [https://www.xda-developers.com/whatsapp-updates-terms-privacy-policy-mandate-data-sharing-facebook/](https://www.xda-developers.com/whatsapp-updates-terms-privacy-policy-mandate-data-sharing-facebook/).
23. Kseniia Ermoshina, Francesca Musiani, Harry Halpin. End-to-End Encrypted Messaging Protocols:An Overview. Third International Conference, INSCI 2016 — Internet Science, Sep 2016, Florence, Italy. pp.244–254. [https://hal.inria.fr/hal-01426845/file/paper_21.pdf](https://hal.inria.fr/hal-01426845/file/paper_21.pdf).
24. A look at Matrix.org’s OLM | MEGOLM encryption protocol. [https://blog.jabberhead.tk/2019/03/10/a-look-at-matrix-orgs-olm-megolm-encryption-protocol/](https://blog.jabberhead.tk/2019/03/10/a-look-at-matrix-orgs-olm-megolm-encryption-protocol/).
25. Telegram publishes users’ locations online. [https://blog.ahmed.nyc/2021/01/if-you-use-this-feature-on-telegram.html](https://blog.ahmed.nyc/2021/01/if-you-use-this-feature-on-telegram.html).
26. Five Reasons You Should Delete Telegram from Your Phone. [https://www.vice.com/en/article/jgqqv8/five-reasons-you-should-delete-telegram-from-your-phone](https://www.vice.com/en/article/jgqqv8/five-reasons-you-should-delete-telegram-from-your-phone).
27. Why is Signal asking users to set a PIN, or “A few thoughts on Secure Value Recovery” [https://blog.cryptographyengineering.com/2020/07/10/a-few-thoughts-about-signals-secure-value-recovery/](https://blog.cryptographyengineering.com/2020/07/10/a-few-thoughts-about-signals-secure-value-recovery/).
28. German police can access any WhatsApp message without any malware. [https://androidrookies.com/german-police-can-access-any-whatsapp-message-without-any-malware/](https://androidrookies.com/german-police-can-access-any-whatsapp-message-without-any-malware/).
29. What’s wrong with WhatsApp. [https://www.theguardian.com/technology/2020/jul/02/whatsapp-groups-conspiracy-theories-disinformation-democracy](https://www.theguardian.com/technology/2020/jul/02/whatsapp-groups-conspiracy-theories-disinformation-democracy).
30. Signal secure messaging can now identify you without a phone number. [https://nakedsecurity.sophos.com/2020/05/22/signal-secure-messaging-can-now-identify-you-without-a-phone-number/](https://nakedsecurity.sophos.com/2020/05/22/signal-secure-messaging-can-now-identify-you-without-a-phone-number/).
31. WhatsApp gives users an ultimatum: Share data with Facebook or stop using the app. [https://arstechnica.com/tech-policy/2021/01/whatsapp-users-must-share-their-data-with-facebook-or-stop-using-the-app/](https://arstechnica.com/tech-policy/2021/01/whatsapp-users-must-share-their-data-with-facebook-or-stop-using-the-app/).
32. Attack of the Week: Group Messaging in WhatsApp and Signal. [https://blog.cryptographyengineering.com/2018/01/10/attack-of-the-week-group-messaging-in-whatsapp-and-signal/](https://blog.cryptographyengineering.com/2018/01/10/attack-of-the-week-group-messaging-in-whatsapp-and-signal/).
33. Kevin Lee, Ben Kaiser, Jonathan Mayer. An Empirical Study of Wireless Carrier Authentication for SIM Swaps, January 10, 2020. [https://www.issms2fasecure.com/assets/sim_swaps-01-10-2020.pdf](https://www.issms2fasecure.com/assets/sim_swaps-01-10-2020.pdf).
34. Study finds five major US carriers vulnerable to SIM-swapping tactics. [https://www.engadget.com/2020-01-12-princeton-study-sim-swap.html](https://www.engadget.com/2020-01-12-princeton-study-sim-swap.html).
35. Reverse Engineering WhatsApp Encryption For Chat Manipulation And More. [https://i.blackhat.com/USA-19/Wednesday/us-19-Zaikin-Reverse-Engineering-WhatsApp-Encryption-For-Chat-Manipulation-And-More.pdf](https://i.blackhat.com/USA-19/Wednesday/us-19-Zaikin-Reverse-Engineering-WhatsApp-Encryption-For-Chat-Manipulation-And-More.pdf).
36. Matrix protocol. [https://matrix.org/docs/spec/](https://matrix.org/docs/spec/).
37. Analyzing WhatsApp Calls with Wireshark, radare2 and Frida. [https://medium.com/@schirrmacher/analyzing-whatsapp-calls-176a9e776213](https://medium.com/@schirrmacher/analyzing-whatsapp-calls-176a9e776213).
38. WhatsApp white paper. [https://www.whatsapp.com/security/](https://www.whatsapp.com/security/).
39. Every secure messaging app needs a self-destruct button. [https://techcrunch.com/2019/06/12/every-secure-messaging-app-needs-a-self-destruct-button/](https://techcrunch.com/2019/06/12/every-secure-messaging-app-needs-a-self-destruct-button/).
40. Chinese Cyberattack Hits Telegram, App Used by Hong Kong Protesters. [https://www.nytimes.com/2019/06/13/world/asia/hong-kong-telegram-protests.html](https://www.nytimes.com/2019/06/13/world/asia/hong-kong-telegram-protests.html).
41. Stop Using Encrypted Email. [https://latacora.micro.blog/2020/02/19/stop-using-encrypted.html](https://latacora.micro.blog/2020/02/19/stop-using-encrypted.html).
42. Session: An Open Source Private Messenger That Doesn’t Need Your Phone Number. [https://itsfoss.com/session-messenger/](https://itsfoss.com/session-messenger/).
43. Status.im Whitepaper. [https://status.im/whitepaper.pdf](https://status.im/whitepaper.pdf).
44. Peer-to-Peer Messaging — Where Whisper Falls Short and Waku Picks Up. [https://our.status.im/peer-to-peer-messaging-where-whisper-falls-short-and-waku-picks-up/](https://our.status.im/peer-to-peer-messaging-where-whisper-falls-short-and-waku-picks-up/).
45. WhatsApp Security Flaws Could Allow Snoops to Slide Into Group Chats. [https://www.wired.com/story/whatsapp-security-flaws-encryption-group-chats](https://www.wired.com/story/whatsapp-security-flaws-encryption-group-chats)/.
46. Status.im Specs. [https://status.im/research/secure_messaging_compendium.html](https://status.im/research/secure_messaging_compendium.html).
47. Germany’s data chief tells ministries WhatsApp is a no-go. [https://www.dw.com/en/germanys-data-chief-tells-ministries-whatsapp-is-a-no-go/a-53474413](https://www.dw.com/en/germanys-data-chief-tells-ministries-whatsapp-is-a-no-go/a-53474413).
48. Abusing WebRTC to Reveal Coarse Location Data in Signal. [https://medium.com/tenable-techblog/turning-signal-app-into-a-coarse-tracking-device-643eb4298447](https://medium.com/tenable-techblog/turning-signal-app-into-a-coarse-tracking-device-643eb4298447).
49. We Chat, They Watch. [https://citizenlab.ca/2020/05/we-chat-they-watch/](https://citizenlab.ca/2020/05/we-chat-they-watch/).
50. WhatsApp Beaten By Apple’s New iMessage Privacy Update. [https://www.forbes.com/sites/zakdoffman/2021/01/03/whatsapp-beaten-by-apples-new-imessage-update-for-iphone-users](https://www.forbes.com/sites/zakdoffman/2021/01/03/whatsapp-beaten-by-apples-new-imessage-update-for-iphone-users).
51. Briar Messaging app. [https://briarproject.org/how-it-works/](https://briarproject.org/how-it-works/).
52. I don’t trust Signal. [https://drewdevault.com/2018/08/08/Signal.html](https://drewdevault.com/2018/08/08/Signal.html).
53. Hardware Security Module (HSM). [https://copperhead.co/blog/secure-phone-series-device-security/](https://copperhead.co/blog/secure-phone-series-device-security/).
54. The Great iPwn Journalists Hacked with Suspected NSO Group iMessage ‘Zero-Click’ Exploit. [https://citizenlab.ca/2020/12/the-great-ipwn-journalists-hacked-with-suspected-nso-group-imessage-zero-click-exploit/](https://citizenlab.ca/2020/12/the-great-ipwn-journalists-hacked-with-suspected-nso-group-imessage-zero-click-exploit/).
55. Status Perfect Forward Secrecy Whitepaper. [https://status.im/technical/pfs.html](https://status.im/technical/pfs.html).
56. Peer to Peer Communication Library. [https://libp2p.io/](https://libp2p.io/).
57. Session Protocol: Technical implementation details. [https://getsession.org/session-protocol-technical-information/](https://getsession.org/session-protocol-technical-information/).
58. Signal: Private Group Messaging. [https://signal.org/blog/private-groups/](https://signal.org/blog/private-groups/).
59. Apple’s privacy labels reveals Whatsapp and Facebook Messenger’s hunger for user data. [https://www.techradar.com/news/apples-privacy-labels-reveals-whatsapp-and-facebook-messengers-hunger-for-user-data](https://www.techradar.com/news/apples-privacy-labels-reveals-whatsapp-and-facebook-messengers-hunger-for-user-data).
60. Authoritarianism Through Coding: Signal. [https://www.oyd.org.tr/en/articles/signal/](https://www.oyd.org.tr/en/articles/signal/).
61. Why we need wide adoption of social recovery wallets. [https://vitalik.ca/general/2021/01/11/recovery.html](https://vitalik.ca/general/2021/01/11/recovery.html).
62. Cryptography Dispatches: The Most Backdoor-Looking Bug I’ve Ever Seen. [https://buttondown.email/cryptography-dispatches/archive/cryptography-dispatches-the-most-backdoor-looking/](https://buttondown.email/cryptography-dispatches/archive/cryptography-dispatches-the-most-backdoor-looking/).
63. On Privacy versus Freedom. [https://matrix.org/blog/2020/01/02/on-privacy-versus-freedom](https://matrix.org/blog/2020/01/02/on-privacy-versus-freedom).
64. Michael Egorov, MacLane Wilkison, David Nunez. NuCypher KMS: Decentralized key management system, November 15, 2017. [https://arxiv.org/pdf/1707.06140.pdf](https://arxiv.org/pdf/1707.06140.pdf).
65. Bridging Matrix with WhatsApp running on a VM. [https://matrix.org/docs/guides/whatsapp-bridging-mautrix-whatsapp](https://matrix.org/docs/guides/whatsapp-bridging-mautrix-whatsapp).
66. Issue 1936: Signal: RTP is processed before call is answered. [https://bugs.chromium.org/p/project-zero/issues/detail?id=1936](https://bugs.chromium.org/p/project-zero/issues/detail?id=1936).
67. Apple can’t read your device data, but it can read your backups. [https://www.theverge.com/2020/1/21/21075033/apple-icloud-end-to-end-encryption-scrapped-fbi-reuters-report](https://www.theverge.com/2020/1/21/21075033/apple-icloud-end-to-end-encryption-scrapped-fbi-reuters-report).
68. Signal outage is keeping messages from sending. [https://www.theverge.com/2021/1/15/22232993/signal-outage-new-users-messages-not-sending](https://www.theverge.com/2021/1/15/22232993/signal-outage-new-users-messages-not-sending).
69. We can do better than Signal. [https://icyphox.sh/blog/signal](https://icyphox.sh/blog/signal/).
70. Fediverse. [https://medium.com/@VirtualAdept/a-friendly-introduction-to-the-fediverse-5b4ef3f8ed0e](https://medium.com/@VirtualAdept/a-friendly-introduction-to-the-fediverse-5b4ef3f8ed0e).
71. Centralisation is a danger to democracy. [https://redecentralize.org/blog/2021/01/18/centralization-is-a-danger-to-democracy](https://redecentralize.org/blog/2021/01/18/centralization-is-a-danger-to-democracy).
72. The Secure Messaging App Conundrum: Signal vs. Telegram. [https://cqi.inf.usi.ch/publications/telegram_vs_signal.pdf](https://cqi.inf.usi.ch/publications/telegram_vs_signal.pdf).
73. The battle inside Signal. [https://www.platformer.news/p/-the-battle-inside-signal](https://www.platformer.news/p/-the-battle-inside-signal).
74. The State of State Machines. [https://googleprojectzero.blogspot.com/2021/01/the-state-of-state-machines.html](https://googleprojectzero.blogspot.com/2021/01/the-state-of-state-machines.html).
75. Signal’s TLS Proxy Failed to be Probing Resistant. [https://github.com/net4people/bbs/issues/60](https://github.com/net4people/bbs/issues/60).
76. Signal’s proxy implementation. [https://github.com/net4people/bbs/issues/63](https://github.com/net4people/bbs/issues/63).
77. Can The FBI Hack Into Private Signal Messages On A Locked iPhone? Evidence Indicates Yes. [https://www.forbes.com/sites/thomasbrewster/2021/02/08/can-the-fbi-can-hack-into-private-signal-messages-on-a-locked-iphone-evidence-indicates-yes/](https://www.forbes.com/sites/thomasbrewster/2021/02/08/can-the-fbi-can-hack-into-private-signal-messages-on-a-locked-iphone-evidence-indicates-yes/).
78. Signal Server is effectively closed source software right now. [https://lemmy.ml/post/55595](https://lemmy.ml/post/55595).
79. WhatsApp and most alternatives share the same problem. [https://stuker.com/2021/whatsapp-and-most-alternatives-share-the-same-problem/](https://stuker.com/2021/whatsapp-and-most-alternatives-share-the-same-problem/).
80. Signal is a government op. [https://yasha.substack.com/p/signal-is-a-government-op-85e](https://yasha.substack.com/p/signal-is-a-government-op-85e).
81. Cryptographers unearth vulnerabilities in Telegram’s encryption protocol. [https://www.cyberscoop.com/telegram-app-security-encryption](https://www.cyberscoop.com/telegram-app-security-encryption).
82. Improved E2E Encryption. [https://www.groupsapp.online/post/improved-e2e-encryption](https://www.groupsapp.online/post/improved-e2e-encryption).
83. Why not Signal? [https://dessalines.github.io/essays/why_not_signal.html](https://dessalines.github.io/essays/why_not_signal.html).
84. interview-with-signals-new-president [https://www.schneier.com/blog/archives/2022/10/interview-with-signals-new-president.html/#comment-411335](https://www.schneier.com/blog/archives/2022/10/interview-with-signals-new-president.html/#comment-411335).
85. I don’t trust Signal [https://blog.dijit.sh/i-don-t-trust-signal](https://blog.dijit.sh/i-don-t-trust-signal).

Note: This was originally published on [http://weblog.plexobject.com/?p=2381](http://weblog.plexobject.com/?p=2381).