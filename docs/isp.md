---
hide:
  - navigation
  - toc
---

# I work for an Internet Service Provider

=== "Starting Out"

    ```mermaid
    flowchart LR
        id1(Customer Reported Issue)
        id2(Is there an existing procedure to follow?)
        id3(Start at Stage 1)
        id4(It's probably bad anyway)
        id5(Ok then)
        click id3 href "#__tabbed_1_2" "Stage 1"
        id1--> id2
        id2-- No -->id3
        id2-- Yes -->id4
        id4-- Yes it is -->id3
        id4-- No it's not --> id5
        id5--> id3
    ```

=== "Stage 1 - Classification"

    !!! warning

        **Never** concede that an issue is due to the network unless advised to do so by engineers _after_ submitting a successful escalation to them, this is an important part of customer expectation management and ensuring that they maintain a cooperative stance with us for long as possible during the troubleshooting process. Once a target for blame has been decided, this can be very difficult to undo and can lead to some very tricky situations if the customer has drawn an incorrect conclusion.

    To classify a technical customer issue as a Support agent for an ISP, there are four "classifiers" you need to define:

    - **Technical classification -** Which Layer in the OSI network model that the issue exists
    - **Scope classification -** The scope of which issues your business will and will not support
    - **Compliance classification -** Is this customer going to bust your ass via some kind of poorly implemented, easy-to-abuse [regulatory scheme?](https://www.tio.com.au/)
    - **Cost classification -** Is it worth supporting this customer in financial and labour-time terms, and are they being supported to the detriment of other customers?

    ### Technical

    Of the OSI model, as an ISP support technician you are only interested in the first 4 Layers:

    - **Layer 1 - Physical (cabling/wifi signal):** Always get the customer to do troubleshooting with a cabled-in connection.
    - **Layer 2 - Data-link (Ethernet protocol):** eg. Is PPPoE or DHCP happening properly?
    - **Layer 3 - Network (IPv4 / IPv6):** ICMP (ping tests) happens at this layer, are they working?
    - **Layer 4 - Transport (Primarily TCP/UDP):** Issues become complex here, but as a general rule - are higher-level applications dependent on Layer 4 protocols working as they should (DNS / HTTP / SSH / FTP / etc)?

    **Examples:**

    - Customer getting high ping times to an IP Address? That's ICMP - Layer 3, so collect Layer 3 troubleshooting information (Customer IP Address, IP Address they get bad response time to, traceroutes, etc)
    - Customer getting no link light on NTD? That's Physical - Layer 1, try a new cable, check the NTD is powered on, check their router is powered on, etc.
    - Customer getting slow downloads? That's a combination of all 4 Layers that need checking:
        - Physical link layer will determine the maximum physical speed possible on the cable
        - Data-link layer will determine the maximum speed possible on the circuit between the ISP and the NTD at the customer premises
        - Network layer will determine the route of the path, which will incur latency that affects TCP performance
        - Transport layer will determine the speed of a transfer based on algorithmic rules, such as TCP congestion control, in an attempt to balance speed with transmission quality.

    The best way to classify these issues is with somewhat of a matrix

    | Issue Layer | Isolation Tested? |
    |------------:|-------------------|
    | Layer 1 OK? | Yes/No            |
    | Layer 2 OK? | Yes/No            |
    | Layer 3 OK? | Yes/No            |
    | Layer 4 OK? | Yes/No            |
    |   Speed OK? | Yes/No            |
    | Latency OK? | Yes/No            |

    ---

    ### Scope

    Is the issue the customer is having within the scope of issues that your company supports as part of the service being sold to them?

    With low-margin services like residential broadband, the answer is almost always unfortunately "no", if the service is working then there is often no financial wiggle-room to assist them beyond the functionality of the service itself, unless the customer is willing to pay extra for that support.

    There are only two classifications to make here:

    - The issue is **in scope**, and will be supported
    - The issue is **not in scope**, and will not be supported

    ---

    ### Compliance

    Some regulatory schemes burden companies with the cost of easy-to-abuse regulatory mechanisms that some customers will use as a pre-emptively punitive measure against service providers. These schemes are becoming only more punitive over time due to the overall increase in service levels across the industry as a whole, resulting in the schemes implementing increasingly difficult-to-comply-with regulations in order to protect their revenue streams. To this end, it's important to be wary of promising, guaranteeing, suggesting or otherwise making any comment about a product that is not already available in the original sales material presented to the customer when they signed up for the service. It's not uncommon for customers to threaten regulatory mechanisms as a "motivator" to fix an issue even if the issue is unrelated to the service, in such situations caution is advised and should be dealt with by an experienced team leader or manager.

    There are two classifications to make here:

    - The customer is **likely** to be a compliance burden
    - The customer is **unlikely** to be a compliance burden

    ---

    ### Cost

    There are many costs associated with supporting customers. Of particular note are the indirect costs of notorious customers in terms of effect on team morale for both support and engineering staff. Many notorious customers stand upon a foundation of poor expectation management, which often stem from their interactions with sales and first-contact interactions with support staff. This is further complicated when the notorious customer is influential or noisy, as a public tantrum from them can become very bad PR for your company.

    **For example:**
    *Very often if a Level 1 support staff member even off-handedly suggests that there is a network issue (even with evidence to the contrary), it immediately spins a very tangled, expensive and complicated web for numerous higher-level staff to untangle - and often the case is that there are no technical issues within scope to solve. An incorrectly handled issue could be escalated to the point of potentially wasting collective weeks of higher-level staff's labour time for ultimately the same outcome that would have been achieved by initially setting and managing customer expectations correctly at the beginning.*

    *Further down this train of thought, imagine this notorious customer is a Twitch streamer, with 1,000 sweaty fans that watched them disconnect from a grand final match due to their pet cat chewing through the router's power supply cable, only for that streamer to blame "this shitty ISP" to those 1,000 watchers because of previously poorly handled expectations that resulted in negative sentiment. There's 1,000 people your company is now a joke to because they can't see the fried cat laying under the desk on stream.*

    There are four classifications to make here:

    - The customer is **high cost, high impact** - they will take a-lot of time to deal with but are vocal brand advocates (hint: Twitch streamers)
    - The customer is **high cost, low impact** - they will take a-lot of time to deal with and are not vocal brand advocates (hint: also Twitch streamers, and most Linus Tech-Tips fans)
    - The customer is **low cost, high impact** - they are not too demanding and are vocal brand advocates (hint: the best and rarest type of customer)
    - The customer is **low cost, low impact** - They are not too demanding and are not vocal brand advoctates (hint: the most common type of customer)

    [Stage 2 - Troubleshooting](#__tabbed_1_3)

=== "Stage 2 - Troubleshooting"

    !!! warning

        **Do not** skip any steps in this process, even if the customer claims to have already completed them. As House M.D. put it so well: **everybody lies**.

    Once an issue has been classified correctly, these classifications provide a guide for following the correct pathway for troubleshooting.

    Some rules that apply in all scenarios:

    - **Never** Wireshark/packet-capture/tcpdump unless directed to do so by engineers _after_ submitting a successful escalation to them, it is exceedingly rare for this to be helpful in low-layer troubleshooting
    - **Never** change the customer's IP Address, or move them between CGNAT and non-CGNAT addressing, unless directed to do so by engineers _after_ submitting a successful escalation to them
    - **Always** follow this guide from Layer 1 through to the end for every problem, as it systematically verifies the functionality of various systems in place
    - **Always** pay attention to the Scope defined for each Layer - you cannot be responsible for issues internal to customer networks, and these issues are certainly unwelcome as an escalation
    - **Always** be aware of the [OSI Model](https://www.redhat.com/en/blog/osi-model-bean-dip), which is where the concept of Layers is derived from

    ### Layer 1 Classification

    **Scope:** Layer 1 between NTD and the Customer's router / isolated test device needs to work.

    - Verify physical link using link-light indicators on NTD and customer router / device
    - Swap cables between customer router and NTD
    - Try a different device connected to the NTD (isolation test)
    - Run Service Health Checks on NTD to check NTD for port errors / faults
    - Record results

    ---

    ### Layer 2 Classification

    **Scope:** Layer 2 between NTD and the Customer's router / isolated test device needs to work.

    - Complete Layer 1 troubleshooting
    - Check for successful DHCP
    - Run Service Health Check for MAC Learning on NTD Port
    - Record results

    ---

    ### Layer 3 Classification

    **Scope:** Layer 3 between Customer's router / isolated test device and valid Internet IP Address

    - Complete Layer 1 and 2 troubleshooting
    - Check for ICMP Echo (Ping) to destination IP Address
    - Check for ICMP traceroute to destination IP Address
    - Record results

    ---

    ### Layer 4 Classification

    **Scope:** Layer 4 between Customer's router / isolated test device and valid service on the Internet

    - Complete Layer 1, 2 and 3 troubleshooting
    - Check for working DNS (to test DNS and UDP)
    - Check for working HTTP/S, Telnet or SSH (to test TCP)
    - Record results

    ---

    ### Speed Issue Classification

    **Scope:** Speed between NTD and the Customer's router / isolated test device

    - Complete Layer 1, 2, 3 and 4 troubleshooting
    - Determine the IPv4 or IPv6 Address of the server being downloaded from
    - Get a return-path traceroute (that is, a traceroute from the slow server back to the customer)
    - Check and compare performance to low-latency server (local Speedtest servers)
    - Record results

    ---

    ### Latency Issue Classification

    **Scope:** Latency between the Customer's router / isolated test device and valid Internet IP Address

    - Complete Layer 1, 2, 3 and 4 troubleshooting
    - Determine the IPv4 or IPv6 Address of the server they are experiencing high latency to
    - Get a return-path traceroute (that is, a traceroute from the high latency destination back to the customer)
    - Check and compare performance to a known low-latency destination (eg. customer's BNG, or local DNS Recursor)
    - Record results

    ---

    That's it - if in this process, you identified and solved the problem, that's great!

    If not but you've followed this process correctly, you have a suitable amount of information to present in an escalation. However there is still more you need to do before escalating.

    [Stage 3 - Expectations](#__tabbed_1_4)

=== "Stage 3 - Expectations"

    Now that you have collected the data, there is one more decision to be made - is this actually a problem?

    - If you have a customer complaining about a small latency jump, say 10ms - are they on a plan that provides guarantees or SLAs latency? Does the wholesale network they're on guarantee latency or pathing for their service?
    - If you have a customer complaining about speed, have their expectations been reasonably managed to understand why they may not always be able to get full speed to everything all of the time?
    - Is it only a problem when they're not isolation-testing?

    Customer expectations must be managed appropriately.

    ---

    ### TCP Performance Expectations

    It's important to consider that packet-loss comes in many forms. One of those forms is transmission reliability, and no transmission medium is capable of 100% successful packet delivery.

    One of the metrics that can be used to predict the minimum level of packet-loss to expect is the Bit Error Ratio (or `BER`).

    |  Access Technology | Bit Error Ratio* |
    |-------------------:|------------------|
    |        FTTP (GPON) | 1e^-9^           |
    |   HFC (DOCSIS 3.1) | 1e^-9^           |
    | FTTN / FTTC (VDSL) | 1e^-7^           |

    **Typical figures - DOCSIS 3.1 is best-case scenario*

    The above table suggests for example that, one in every 1 billion bits on FTTP will be errored. These values could be considered the absolute minimum level of packet-loss that occurs at any time on any one internet connection. In reality it is possible that these mediums could be more or less reliable than what is stated in this table, however these are all values that could be considered within normal operating ranges.

    At 1Gbps this suggests one bit error per second, which TCP may see as packet-loss due to an errored packet if not recognised and dropped by a lower level protocol (in which case it will be seen as packet-loss anyway!) and either way will result in a reset of the TCP Window Size. At higher latencies, this reset is quite expensive and results in a significant reduction of throughput.

    One important calculation for predicting TCP Performance is the **B**andwidth **D**elay **P**roduct. This is calculated as `bandwidth` x `round-trip time`.
    The idea behind this calculation is that the TCP Window Size needs to be at-least the size of the calculated BDP, otherwise not all the bandwidth will be able to be used since transmission time will be spent waiting for TCP ACK packets instead of transferring data.

    A step further we can go is the *Mathis* equation. This predicts the throughput of TCP taking into account latency and packet-loss, but not including overheads, with the following equation:

    **MSS / (RTT x &radic;Loss Rate)** = *Throughput*

    - MSS is expressed in bits (eg. **1460 bytes** is **11680 bits**)
    - RTT is expressed in seconds (eg. **30 ms** is **0.03 seconds**)
    - Loss Rate is expressed in scientific notation or decimal (eg. **1e^-3^** is **0.001**)
    - Throughput is expressed as bits per second

    **Example:**

    **11680 / (0.03 x &radic;0.001)** = *12,311,801.02 bps*

    We know our absolute minimum Loss Rate is equal to the Bit Error Ratios for the access mediums above - but it is important to remember that every bit sent or received on the internet is subject to this probability on every single link it traverses, not all links are always performing perfectly all of the time, etc.

    Below is a table showing various maximum attainable TCP throughput for difference combinations of latency and loss. It only takes the smallest amount of intuition to realise that the idea of consistent 99.999999999% (**1e^-9^ Loss Rate**) packet delivery between two distant locations such as Australia and Europe over the general internet is not a reasonable expectation.


    | **RTT(ms)** | **BER(Loss Rate)** |            |            |            |            |            | **MSS: 11680** |
    |------------:|--------------------|------------|------------|----------- |------------|------------|----------------|
    |             | **1e^-3^**         | **1e^-4^** | **1e^-5^** | **1e^-6^** | **1e^-7^** | **1e^-8^** | **1e^-9^**     |
    |    **10ms** | 36.94Mbps          | 116.80Mbps | 369.35Mbps | 1,168Mbps  | 3,693Mbps  | 11,680Mbps | 36,935Mbps     |
    |    **30ms** | 12.31Mbps          | 38.93Mbps  | 123.12Mbps | 389.33Mbps | 1,231Mbps  | 3,893Mbps  | 12,311Mbps     |
    |   **100ms** | 3.69Mbps           | 11.68Mbps  | 36.94Mbps  | 116.80Mbps | 369.35Mbps | 1,168Mbps  | 3,693Mbps      |
    |   **300ms** | 1.23Mbps           | 3.89Mbps   | 12.31Mbps  | 38.93Mbps  | 123.12Mbps | 389.33Mbps | 1,231Mbps      |


    And to aid reading above

    | **Scientific Notation** | **Decimal** |
    |------------------------:|-------------|
    |                  1e^-3^ | 0.001       |
    |                  1e^-5^ | 0.00001     |
    |                  1e^-7^ | 0.0000001   |
    |                  1e^-9^ | 0.000000001 |

    ---

    #### High Latency TCP Performance on VPNs

    Many VPN's implement their own flavours of "TCP Acceleration" mechanisms. There are various proxying techniques available for TCP that can be leveraged on a VPN - some of these techniques are quite hacky but do work to improve performance. To research further, you can research techniques such as *TCP Synproxy*.

    The important thing to note is that these VPN's effectively split one high-latency TCP path into two or more lower-latency TCP paths. The tables above demonstrate that this will have a clearly positive effect on attainable speeds so long as the Loss Rate does not increase significantly. Sometimes these acceleration techniques can result in interesting behaviour, such as certain games showing the latency between the VPN Server and the Game Server, or the VPN Server and the Game Client, rather than the latency between the actual Game Client and the Game Server. This is very easy to spot when the latency is reported at levels low enough to be impossible under the laws of physics that govern the various transmission and signalling mediums involved, many end-users do not seem to understand or grasp this unfortunately.

    When customers complain that TCP performance improves over VPN services, it is fine to simply tell them that this is expected behaviour.

    [Stage 4 - Escalation](#__tabbed_1_4)

=== "Stage 4 - Escalation"

    By this stage, **all** of following is true:

    - You have classified the technical nature of the problem (Layer 1 / 2 / 3 / 4 / Speed / Latency)
    - You have classified the scope of support and whether the issue falls within the scope of support
    - You have classified the customer in terms of whether or not they are likely to be a compliance burden
    - You have classified the cost of the customer in terms of how demanding they are, and their impact/influence
    - You have appropriately troubleshooted the issue and have been unable to resolve the problem
    - You have appropriately managed customer expectations
    - You have recorded the results of troubleshooting
    - You have not performed packet-captures or other advanced troubleshooting techniques

    **If you can say yes to all of the above, you're officially ready to escalate an issue to the engineers!**

    ---

    ### Submitting an Escalation

    Submit an escalation in a well presented way, engineers do not need or want to see all the chit-chat between support agents and the customer, it consumes valuable time.

    **Do:**

    - Provide a summary of the issue the customer is experiencing in it's current state
    - Submit a summary of the classifications made (Technical, Scope, Compliance and Cost)
    - Submit results of the troubleshooting performed in a well organised fashion

    **Do not:**

    - Put the customer in direct contact with the escalation engineer unless advised to do so by the escalation engineer
    - Make admissions of fault to the customer during the escalation process unless advised to do so by the escalation engineer
    - Submit an escalation containing comms between Support Agents and Customers, you should be summarising the relevant information as mentioned above
    - Silo internal communications regarding the case into Direct or Private Messages of any kind, as this significantly detracts from the ability to reassign cases and share knowledge relevant to the cases effectively