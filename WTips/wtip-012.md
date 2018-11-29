---
wtip: TBD
title: ENS Integration
authors: Augusto Lemble <augusto@windingtree.com>
status: Draft
created: 2018-29-11
---

WTip TBD

## Simple Summary
Assign an ENS sub domain to each hotel registered in the WT index, an example url will be `awesome-hotel.windingtree.ens`.

## Abstract

[ENS](https://ens.domains/) provides a more readable way of representing unique identifiers in a network, it comes with an infrastructure that allow WT to assign multiple resolvers for a domain.

The [WTIndex contract](https://github.com/windingtree/wt-contracts/blob/91edcd730cc702d211672853e873eb1530895344/contracts/WTIndex.sol) is a resolver of URIs for hotels, by knowing the address of a hotel the user can get the URI to the hotel's root document, the following proposal describes how this can be replaced by using ENS with a public resolver and the benefits of it.

## Motivation

Every hotel or airline registered in WT will have their own domain like they have right now in the WWW, this is something that WT needs in order to built a decentralized platform for the travel industry.
These unique identifiers can be used in APIs, signatures verification, cross verification with DNS domains, hotel explorer, payments.

## Specification

This integration will affect the `registerHotel` `deleteHotel` `callHotel` and `transferHotel` methods.
Each hotel will have a [nameHash](http://docs.ens.domains/en/latest/implementers.html#namehash), which would be able to be deleted, called or transferred.
The `registerHotel` function will also receive a string parameter that will be used to generate nameHash for the hotel subdomain and assign the ownership to the hotel manager.
On methods `deleteHotel` `callHotel` and `transferHotel` the `hotelAddr` parameter will be replaced for `nameHash`.

The proposal also includes the creation of a WTResolver contract, this will be used by hotels and airlines, simplifying the contract code and standardizing the distribution and access to content in the network.

![Diagram - overview](../assets/wtip-xxx/WT%20ENS.png)

### WT Index Contract

The hotel contract will be removed and the WTIndex can register different types of domains like hotels, airlines, travel companies, payment providers, personal accounts, mostly anything.

The WTIndex contract will be owned by the WT main multisignature wallet and it will manage the ownership of the windingtree.eth subdomains.
The domains can enable recovery and shared management features, this feature can have a cost per X amount of blocks, but is intended to be free at the beginning.

The WTIndex contract will also have types of domains, the domains can be: hotel, airline or personal. More domain types can be added in the future and only WT have permissions for it.

### WT Resolver Contract

The WTResolver contract will be based on the [PublicResolver](https://github.com/ensdomains/ens/blob/master/contracts/PublicResolver.sol) contract that is being used on ENS. It will have store the following type of records:
- Name, string type.
- Address, address type.
- Text, string type.
- URI, string type.
- Content, bytes32 type.
- Public key, SECP256k1 public key, saving x and y coordinates of the curve point of the public key in bytes32.

## Backwards Compatibility
This changes will affect the libraries and APIs built around the WT Index contract, the hotels will be identified by domain name or nameHash instead of an address. This will be the only difference with the current implementation

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
