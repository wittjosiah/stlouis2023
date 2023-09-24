Notes from identity / auth session

- Questions:
 - Are there a general set of requirements for security?
    - If we don't have that, we can't do anything
 - Radical security - Empowering people to protect each other, provide for each other
 - Security exploits at the level of standards and historical exploits
 - looked at automerge and wanted to work with it, but wasn't sure how to model the authorization in less than fully trusted relations
 - How do we know who to trust, and how do we know that we know their info is correct?
- Are there requirements?
 - "I For sure don't want a global identifier, because it sounds dystopian"
 - "I think we for sure need that"
    - We need to be able to identify people, email already is basically this, Know Your Customer requirements,
    - Even more importantly, people need to be able sign in again. Especially from multiple devices. Bad experience, bad for trust.
  - "It sounds like the requirement is 'we need key recovery'"
    - e.g. passkeys
  - "Passkeys are global identifiers, only recoverable ones are from google, apple, etc. This centers power in google / apple"
  - "Bluesky solved this partly by having it all based on a key, but managing that key for you. "
  - "0 percent of users want this."
  - "Until they need to."
  - "Even still, doesn't apply. See: facebook, 2 billion users"
  - "But their most popular app is an end to end encrypted messaging app. Takeaway: people do care about it, but not enough to screw up their workflow. Need to make sure it's sufficiently convenient to use."
  - "Maybe not a major differentiator, but it's a long term survivability thing."
  - "Hardcore security should be optional, progressive, and possible so that people can use it when they need it."
- Moving on to talking in terms of requirements:
  - "End to end encryption is a requirement for local first software so that intermediaries are forced to be dumb pipes."
  - Fission does this, and encrypted CRDTs need to be different than non (I think). Need to send more data to handle this for different receivers. Downside is most people use ACLs, which don't fit with CRDTs without weird edge cases.
 - What are Capabilites?
 - "ACLS are basically like a bouncer with a list of names. Capabilities are more like a ticket to go to the movies. No one cares who it is, but everyone knows that *someone* is allowed to go to the movies."
 - "Attentuation is also really important, basically restricting the capabilities to a subset of the original capabilities."
 - "Important part is that simply having a capability is proof that you can do this, you don't need to ask someone else."
 - "Capability semantics give you another layer to provide an API"
 - "Fission's capabilities have a lot of cool features, e.g. being able to pass a key for a top of the tree and get all the sub trees. the CRDT is a pair of the data and a SET of capabilities that are just merged. Merkelization is used primarily for diffs, if you have another mechanism is useful".
- So, in summary, you need PKI for the encryption at rest and E2E, maybe delegating to a trusted party.
 - Last year worked on NNS, like webfinger but decentralized. It's the thing that lets you have mastodon addresses in a federated system. The goal was that your name wouldn't be attached to the instance, but that's how it sorted out. Basically it's a web endpoint that resolves the address.
 - Problem with webfinger: requires a collaborating email server. See: google shutting down their endpoint. NNS basically uses a DHT to use keybase style verification to allow other people to fetch your public key. From there you can bootstrap encryption.
 - What about doing this with entirely ephemeral keys / identities?
 - Subconscious built a system to be compatible with NNS. NNS just takes a key, might be readable might not. This other system uses a key as a lookup into the DHT, and then they use that to identify a wiki of relationships. A transitive pet name system.
 - Zooko's triangle: human readable, secure (unique), decentralized. Pick two.
   - You can get around it by picking any two, and then providing the third with an external pointer.
   - All three are hard requirements because the third will mess up the humans using it.
   - If you don't believe in abstinance, you don't believe in password managers, only 30% of people use them.
   - NNS - you pick your human readable provider.
- Good general requirement: formal verification.
  - Protects against tail risk, not having really good verification => random memory is read -> a lot of your nice properties are null and void.
- Humans are a component in our software systems, how do we inlcude that?
  - all cryptography is breakable in the long term, we shouldn't put *anything* on the network. Even if it's encrypted, on a long enough time scale it's all broken.
  - But we want to do things. So what do we do?
- Going outside is more dangerous than everything we're talking about, how do humans manage that? And what do we do to trust them as elements in our secure systems? Can we even trust them at all?
 - Anecdote: EU: we are going to make it a law to decline cookie storage. Response: this is going to create a lot of popups that are auto-accepted, so it's just friction without any security. People *want* to give up their privacy.
 - People do risky things all the time, and they really want to. So how do we make it so that they can do risky things without compromising them?
 - Example: facebook shared messages to public authorities to out a mom who helped her daughter get an abortion. E2E encryption would have prevented this.
 - The local first orientation is really interesting because it forces us to reckon with these things. Worked on location data sharing, didn't want to share with Yahoo (bad), encrypted the location and account so that no one *could* do it. We have to do these kinds of things in local first software.
- Has anyone built a local first CRDT that does model "we're editing this document and only these X people can do it"
  - "Yeah, by just signing all the changes."