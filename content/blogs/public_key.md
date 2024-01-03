---
title: "Key Exchange"
date: 2019-04-05T00:00:00+01:00
draft: false
#github_link: "https://github.com/gurusabarish/hugo-profile"
author: "ChengSashankh"
tags:
- Diffie-Helman Key Exchange
- Cryptography
- Security
- Asymmetric Encryption
#image: /images/autodesk.png
description: ""
toc:
---

# Creating a Shared Secret

In the previous section, we discussed the benefits of encrypting communications for the benefit of privacy. While this might seem sufficiently secure at a first glance, we will see that the practicalities of the real world demand the specification of a number of other schemes and processes in order to enable this encryption.

For example, the scheme discussed above (referred to as **symmetric encryption**) require Alice and Bob to share a secret key that no other party is aware of. It is unclear so far as to how such a secret may be generated in the event that the parties cannot agree beforehand on such schemes through some other means. In this section, we discuss this problem further, and open a broader discussion on the practicalities of symmetric encryption.

## Motivation

Let us begin again by characterizing the problem that motivates this discussion. Suppose that Alice and Bob are in different parts of the world, and wish to communicate without anyone else eavesdropping on their communications. Recall from the previous section that they can use symmetric encryption to do so, provided they somehow agree upon a secret key without others realizing this key. This requires that they first negotiate a secret key, before communicating. The problem now distills to the following:

> Given that a third party can listen in on all possible communications between Alice and Bob, how can they agree upon a common secret that others do not know about?

In the context of the internet, it is possible that Alice and Bob have never met before (Alice could be a server in different country). It is hence not possible, or practical to rely on shared secret information to create a key. We must create a secret in plain sight.

## Approach

The following is a brief description of the approach. Please note that this section compromises some mathematical accuracies in the interest of simplicity.

In 1976, researchers proposed the Diffie-Hellman protocol to address this problem. The approach to solving this problem begins in a seemingly disconnected mathematical property of numbers: `that under some conditions, it is easy to calculate the exponents of numbers, but  much harder to reverse this process`. This means that in certain groups,

- calculating g<sup>x</sup> given `g` and `x` is a simple problem, and,
- calculating `x` given `g` and g<sup>x</sup> is a much harder problem

(ignore the semantics of the mathematical structure `Group` if it is not relevant to you)

It can now be seen how this mathematical property can be used to create a shared secret. Observe the following interaction. In this discussion, Alice and Bob are attempting to negotiate a secret key and Eve is attempting to listen in on the conversation and steal the key:

- Alice randomly selects a number `x` and calculates g<sup>x</sup>. As we discussed before, this can be achieved easily since it is efficiently calculable. She tells Bob, "Hi, I'd like to chat and here are two numbers g<sup>x</sup> and g."
- Eve can hear this, and she writes down both g and g<sup>x</sup>
- Bob notes down the same details. He now samples a random number y, and similarly calculates g<sup>y</sup>. He tells Alice, "Hi, here are my two numbers g and g<sup></sup>"
- Eve notes these down as well
- Alice receives this and notes down as well.

At this point, each party has the following details:

- Alice: x, g, g<sup>x</sup> and g<sup>y</sup>
- Bob: y, g, g<sup>y</sup> and g<sup>x</sup>
- Eve: g, g<sup>x</sup> and g<sup>y</sup>

Suppose they each attempt to calculate g<sup>xy</sup>:
- Alice can raise g<sup>y</sup> to the power x and obtain g<sup>xy</sup>
- Bob can raise g<sup>x</sup> to the power y and obtain g<sup>yx</sup>, which is the same as g<sup>xy</sup>
- Even cannot calculate g<sup>xy</sup> directly, without first calculating either x or y. As we discussed earlier, the problem of obtaining x or y from g<sup>x</sup> or g<sup>y</sup> respectively is considered very hard if the suitable g is chosen.

Hence, Alice and Bob can generate a shared secret key in plain sight. Remember, these properties are not true of all numbers. They are exclusively true of some mathematical groups, and a further discussion on this would be non-trivial.


The above protocol allows use to now see completely how two unrelated parties can communicate a shared secret key in the presence of malicious actors. They first employ this key-exchange protocol to obtain the shared secret key g<sup>xy</sup> and then proceed to use that to symmetrically encrypt all communications.

The practicality of symmetric encryption is now vastly extended. It can be possible for two parties who have never met before, to communicate confidentially without having to share any secrets beforehand. This brings the simplicity of symmetric encryption to use in the era of the internet.

Even with this protocol, a multitude of issues remain unresolved and un-discussed. We will further our discussion of secure communication in later posts.
