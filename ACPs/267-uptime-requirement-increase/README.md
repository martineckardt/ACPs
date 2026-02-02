| ACP | 267 |
| :--- | :--- |
| **Title** | Increase Validator Uptime Requirement from 80% to 90% |
| **Author(s)** | Martin Eckardt ([@martineckardt](https://github.com/martineckardt)) |
| **Status** | Proposed |
| **Track** | Standards |

## Abstract

This proposal increases the minimum uptime requirement for Avalanche Primary Network validators from 80% to 90%. Validators must maintain at least 90% uptime during their staking period to receive their validation rewards. This change aims to enhance overall network health, reliability, and performance by ensuring validators meet a higher standard of availability.

## Motivation

### Current State

The Avalanche Primary Network currently requires validators to maintain a minimum of 80% uptime during their staking period to be eligible for staking rewards. While this threshold has served the network adequately, there are compelling reasons to raise the bar.

### Need for Enhanced Network Reliability

Higher validator uptime is essential for achieving further performance gains across the Avalanche
Primary Network. Sustained validator availability ensures the network consistently processes
transactions at optimal speed and minimizes bottlenecks caused by unresponsive nodes.

## Specification

### Technical Changes

Update the validator uptime requirement from 80% to 90% by changing the following in `MainnetParams` (defined in [`genesis/genesis_mainnet.go`](https://github.com/ava-labs/avalanchego/blob/master/genesis/genesis_mainnet.go)):

```go
StakingConfig: StakingConfig{
	UptimeRequirement: .9, // 90%
}
```

The same change would be applied to `genesis/genesis_fuji.go` and `genesis/genesis_local.go` for consistency across all networks.

### Implementation Details

The proposed change only raises the validator uptime requirement to 90%, with the uptime calculation
method remaining unchanged. Validators are measured by their observed responsiveness during the
staking period. Validators and their delegators will only receive rewards if the validator achieves
at least 90% uptime; otherwise, they receive no rewards, maintaining the current all-or-nothing
reward model with no partial payouts.

### Impact on Existing Validators

Uptime is tracked continuously throughout a validator's staking period and evaluated at the end when reward eligibility is determined. If this change activates mid-staking-period, validators would have their total accumulated uptime (from their original start time) compared against the new 90% threshold when their staking period ends.

To give validators time to improve their infrastructure if needed, this ACP should be advertised broadly in the community.
