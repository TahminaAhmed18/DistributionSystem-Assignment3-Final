# ⭐️ **DISTRIBUTED SYSTEMS ASSIGNMENT 3 - Paxos Based Adelaide Suburbs Council Election** ⭐️

## 🌟 **Tahmina Ahmed - a1938593** 🌟

---

## **🌟 Assignment Brief 🌟**

The Adelaide Suburbs Council is conducting elections for its council president. Any of the nine council members can become the president. Members, however, have varied responsiveness and preferences:

- **✨ `Member M1`**: Very responsive, always online.
- **✨ `Member M2`**: Poor internet connection; occasionally instant responses at Sheoak Café.
- **✨ `Member M3`**: Occasionally unreachable due to camping trips in remote areas.
- **✨ `Members M4-M9`**: Neutral participants with varying response times.

On the election day, a council member nominates a presidential candidate, and the majority vote determines the president. The goal was to implement a **Paxos-based election system** that is **fault-tolerant** and handles communication delays and disruptions. **Socket-based communication** was utilized for messaging.

---

## **🌟 The Paxos Protocol 🌟**

Paxos is a consensus algorithm designed to achieve agreement within distributed systems, even in the face of partial system failures. The protocol operates in distinct phases, ensuring nodes in a distributed system agree on a single piece of data or value.

### **✨ Overview of Paxos System ✨**
1. **🌟 Initiating Proposal**: A proposer forms a unique proposal ID and queries acceptors on whether a higher ID proposal exists. If not, they propose a value.
2. **🌟 Consensus**: Reached when the majority of acceptors commit to the proposer’s proposal.

### **🌟 Detailed Stages 🌟**
#### **⭐ Stage 1: Proposer (PREPARE) & Acceptor (PROMISE)**
- Proposers generate a unique, ascending ID and send a `PREPARE` request.
- Acceptors:
  - Respond with a `PROMISE` if the proposal ID is the highest.
  - Include previously accepted values in the `PROMISE`.

#### **⭐ Stage 2a: Proposer (PROPOSE)**
- Collects `PROMISE` responses from the majority:
  - Proposes the most recent accepted value or a new value.
- Sends an `ACCEPT` request with the ID and value.

#### **⭐ Stage 2b: Acceptor (ACCEPT)**
- Acceptors:
  - Accept and broadcast the proposal with the highest ID.
  - Otherwise, ignore or decline lower proposals.

Paxos ensures unique proposals and steers the system to agree on a single value, enabling reliable application or recording.

---

## **🌟 Member Class Overview 🌟**

### **✨ Functions**
1. **⭐ Identification & Interaction**: Distinct `memberId` for each Member and communication with peers.
2. **⭐ Proposal Tracking**: Generates and tracks proposal IDs and observed proposals.
3. **⭐ Vote Processing**: Uses `VoteServer` to process and retain accepted proposals.
4. **⭐ Simulating Delays**: Introduces real-world or test condition delays.
5. **⭐ Offline Simulation**: Simulates network issues or failures.
6. **⭐ Consensus Messaging**: Manages messages related to achieving consensus.

---

## **🌟 Server Class Overview 🌟**

### **✨ Functions**
1. **⭐ Achieving Consensus**: Implements Paxos to coordinate voting.
2. **⭐ Communication**: Manages `PREPARE`, `PROMISE`, `ACCEPT`, and `ACCEPTED` messages.
3. **⭐ Member Oversight**: Tracks participating nodes and their states.
4. **⭐ Timeout Handling**: Manages proposal deliberation timeouts.
5. **⭐ Electing a Leader**: Determines the council president via majority votes.

---

## **🌟 Testing 🌟**

### **✨ Paxos Tests**
1. **⭐ `testConcurrentVotingProposals`**
   - Tests simultaneous proposals from multiple members.
   - Verifies the system resolves conflicts and elects the correct president.
   
2. **⭐ `testImmediateResponse`**
   - Validates Paxos behavior under immediate responses.
   - Ensures multiple proposals are handled correctly.

3. **⭐ `testM2orM3GoOffline`**
   - Simulates scenarios where members go offline mid-process.
   - Verifies the system's ability to handle failures and elect a president.

### **✨ VoteServer Tests**
- **⭐ Member Management**: `testSetMembers`, `testAddMember`.
- **⭐ Communication Setup**: `testSetCommunication`.
- **⭐ Message Broadcasting**: `testBroadcast`.
- **⭐ Proposal Comparison**: `testCompareProposalNumbers`.
- Ensures robustness and correctness of `VoteServer` functionalities.

### **✨ Member Tests**
- **⭐ Proposal Management**: `testGenerateProposalNumber`, `testSetAndGetHighestSeenProposalNumber`.
- **⭐ Message Handling**: `testSendPrepareRequest`, `testSendPromiseWhenOnline`, `testSendRejectWhenOffline`.
- **⭐ Delay Simulation**: `testDelayTimeInitialization`, `testSetDelayTime`.
- Validates member-specific functionalities and adherence to the Paxos protocol.

---

## **🌟 run and test my code, detailing all necessary steps. 🌟**

### **✨ Prerequisites**
1. **Java**: Ensure JDK 19 or higher is installed.
2. **Make**: Install `make` for simplified build commands.
---

### **✨ Compiling**
To compile the main application:
```bash
make

##**compile:**
javac -d bin/ src/main/java/CouncilMember.java src/main/java/PaxosServer.java src/main/java/RequestHandler.java src/main/java/tests/Test.java src/main/java/tests/LargeDelayTest.java src/main/java/tests/m2andm3GoesOfflineTest.java src/main/java/tests/m2andm3GoesOfflineTest.java src/main/java/tests/MinorityFailureTest.java src/main/java/tests/SmallDelayTest.java src/main/java/tests/TwoCouncillorsTest.java

##**clean:**
	rm bin/main/java/*.class && rm bin/main/java/tests/*.class

**server:**
	java -cp bin main.java.PaxosServer

**largedelaytest:**
	java -cp bin main.java.tests.LargeDelayTest > ./logs/LargeDelayTestOutput.txt 

**m2m3offlinetest:**
	java -cp bin main.java.tests.m2andm3GoesOfflineTest > ./logs/m2andm3GoesOfflineTestOutput.txt 

**m2offlinetest:**
	java -cp bin main.java.tests.m2GoesOfflineTest > ./logs/m2GoesOfflineTestOutput.txt 

**minorityfailuretest:**
	java -cp bin main.java.tests.MinorityFailureTest > ./logs/MinorityFailureTestOutput.txt 

**smalldelaytest:**
	java -cp bin main.java.tests.SmallDelayTest > ./logs/SmallDelayTestOutput.txt 

**twocouncillorstest:**
	java -cp bin main.java.tests.TwoCouncillorsTest > ./logs/TwoCouncillorsTestOutput.txt
