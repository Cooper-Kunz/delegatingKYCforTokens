# Delegated KYC Policies in HTS

- hip: 28
- title: Delegated KYC Policies in HTS
- author: Cooper & Paul @ Calaxy 
- type: Standards Track
- category: Application
- status: Draft
- created: 2021-10-10
- discussions-to: 
- updated: 

## Abstract

This HIP proposes an extension to the HTS mechanism by which the KYC status of a given Hedera account for a given HTS token can be indicated. Rather than actively managing the KYC status of accounts (using their own KYC key to set the KYC flag on an account [1, 2]), a token admin can delegate that burden to  and responsibility to the admin of some other token. If and when the admin of that delegatee token sets the KYC status of a given account to 'true', the delegator token effectively inherits that status. 

## Motivitation

When creating (or subsequently modified if the token is not immutable) a token on HTS, an admin can optionally stipulate a KYC key. If such a key is stipulated, then individual Hedera accounts will only be able to send or receive that token if that account's KYC flag for that token has been set to true. The current model is that the KYC status for a given Hedera account for different tokens is independent, that is, the admin of each different token is expected to separately manage the KYC status of each account for their own token. 

This HIP proposes that, for a given Hedera account, one token would be able to effectively inherit the KYC status of another token and so remove from the delegator token the burden of actively managing KYC status for their token.

## Rationale

The current model of each token managing KYC status for a given Hedera account separately implies

- a KYC management burden on all tokens that may be difficult for some token admins to assume. 
- possibly redundant KYC checks on the owners of Hedera accounts. Even if an account owner has gone through a KYC check for one token, that fact and implicit approval may not be transferable to another token - with associated negative usability.

## Specification

When creating or modifying a token, admins can optionally stipulate a 'KYCTarget' parameter with a value of an existing token identifier. 

If a token delegates KYC status to another token, the delegator token MUST not have its own KYC key.

If a token delegates KYC status to another token, the delegatee token MUST have a KYC key.

If a token delegates KYC status to another token, Hedera nodes MUST interpret the KYC status of the delegator token as identical to that of the delgatee token.

If a token does not delegate KYC status to another token, .....

## Backwards Compatibility

There are no known backwards comatibility considerations that'd impact implementation. 

All previously created tokens could retain their existing KYC configurations, and those enabled with controlled mutability[3] (i.e. admin KYC keys) could update to stipulate a KYCTarget to delegate the management to.

## Security Implications

Security implications are generally restricted to application-layer considerations, such as - what if you delegate KYC to an entity who then improperly manages their own process(es)? By definition, when delegating the "KYCTarget", a token issuer is delegating their own "KYC Key" security/management to another entity. Thus application developers would want to ensure they have a good relationship, fallback considerations, etc., when choosing a delegate.  

## How to Teach This
Instead of each application developer having to stand up a sufficient verification system, they can "piggyback" onto others. 

For example, if a Hedera Governing Council member eventually offered identity verification services on-chain for a token, other projects/tokens could inherit (e.g. piggyback on) their identity verification. 

## Reference Implementation
TBD

## Rejected Ideas
N/A

## Open Issues
N/A

## References
1. [Hedera documation - Enable KYC account flag](https://docs.hedera.com/guides/docs/sdks/tokens/enable-kyc-account-flag-1)
2. [Hedera documentation - Disable KYC account flag](https://docs.hedera.com/guides/docs/sdks/tokens/disable-kyc-account-flag)
3. [Hedera blog - Controlled Mutability, by Paul Madsen](https://hedera.com/blog/code-is-law-but-what-if-the-law-needs-to-change)
