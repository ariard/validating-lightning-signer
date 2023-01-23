# Validating Lightning Signer Intro

While the Lightning Network is still in nascency phase, the understanding of the new security
paradigm underpinning Lightning channels has made steady progress across the community during
the last years. At its core, a channel can be seen as a private contract between 2 participants,
where any dispute on the valid state is arbitrated by the Bitcoin blockchain. This contract is
made of a chain of pre-signed Bitcoin transactions, kept off-chain and novated lively on the
request of the participants. This model implies strong requirements on the participants, notably
the ability to confirm Bitcoin transactions as a claim proof on the valid channel state before
the expiration of a time window period.

If the channel targets live operations, where a payment can be sent or received at any time,
or a HTLC forwarded for the account of another participant, the signing keys of the channel
transactions should stay on a hot wallet, i.e a computing host exposed to the Internet. Moreover,
in most of today's default Lightning routing nodes deployment this "hot" wallet can be peered
by any external, unknown parties, as there is an incentive to extend its local channels connection
graph to earn higher routing fees on forwarded HTLC traffic. In layman's terms, imagine that
anyone could open a payer-payee relationship with your corporate account, and from then issues
requests on your remaining credit lines with other business partners. Put it simply, a nightmare
from the perspective of modern banking security engineering.

The Internet has not been without the existence of "hot keys" infrastructure during the past decades,
in the presence of [X.509 certificates](https://www.rfc-editor.org/rfc/rfc5280.html) signing authority
underpinning the TLS infrastructure. By standard practice, such signing modules are hosted on so-called
"Hardware Security Modules" with highly-constrained network connectivity, to protect against keys leaks
or signing misusage. While a compromise of a certificate chain can have major repercussions on the
HTTPS traffic under protection, an analogous compromise for a Lightning channel transaction signing keys
can directly translate as a loss of Bitcoin funds.

Therefore, the solution to the Lightning "hot keys" exposure security risk is to isolate the signing
module in a secure and controlled environment, in the similar way of what has been achieved for the
Internet key infrastructure, which should be a significant step forward in securing the Lightning
ecosystem as a whole. To achieve this goal, two approaches could be considered. The first one could
be to isolate a Lightning signer on a hardened computing environment from the main Lightning node,
where the channel transactions are blindly-signed. The main Lightning node would still be responsible
to fulfill payments or HTLC forward requests, routing, p2p network communications, chain access, etc.
However this "naive" approach would increase the [attack surface](https://gitlab.com/lightning-signer/docs/-/wikis/Blind%20Signing%20Considered%20Harmful) of a
Lightning node, by just offering a second point of exploitation.

A second approach has been pursued by the [Validating Lightning Signer project](https://vls.tech), where
the transition of the Lightning state machine ensuring funds safety has been replicated in a [set of multiple policies](https://gitlab.com/lightning-signer/validating-lightning-signer/-/blob/main/docs/policy-controls.md), many of them requiring the maintenance of a channel micro-state. The richness of the policies scope
should enable an automated flow of Lightning operations, where human operators are delegated to a
monitoring role.
