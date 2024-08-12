Certainly! Here’s a revised version of your content with improved tone, organization, and grammar, making it more suitable for a natural presentation. I've also corrected and clarified some information to ensure accuracy.

---

### There are three primary architectures for provisioning storage in an enterprise:

1. **The Server-Centric IT Architecture**
2. **The Storage-Centric IT Architecture**
3. **Information-Centric Architecture**

The latter, Information-Centric Architecture, is the more prevalent, supplanting the former two. However, this video is about the penultimate, Storage-Centric IT Architecture.

Before we move forward, keep in mind that the volume of data an IT infrastructure may need to store and process can be in the realm of petabytes—not commonly feasible and accessible to a common consumer. That’s why large data centers exist to store and process such vast amounts of data.

Take Google, for example. It’s inconceivable to imagine that Google stores and processes the volume of data you and I generate on commonly purchasable storage devices such as USB drives, SSDs, hard disks, or even a combination of these resources. Even if that were the case, managing these independent or jointly connected devices would be an IT nightmare. We are already aware of the challenges to the reliability of the storage media we use for valuable information—sometimes they just break, and we lose important data. This exact issue happened to my partner's sister, who had stored some data on a USB device that is now no longer accessible because it was corrupted after being ejected from the last computer she used. Imagine if Google or a research institution used one of these storage devices to store crucial information, like a cure for cancer or AIDS, and a similar issue occurred. It would be truly tragic to lose such valuable information due to a failed storage medium.

As far back as the 1950s and 1960s, IT infrastructures and enterprises recognized the growing need to store, process, and protect data, making it accessible to end users while safeguarding it from corruption and loss. The first solutions were Directly Attached Storage (DAS) devices. These were popular with mainframe computers and functioned similarly to how we manage our daily data today—creating, updating, and deleting files on SSDs or HDDs of various sizes. These devices could be easily transported if they were in a compact, portable format like a USB stick or SD card but they were not. Most people simply move their laptops, tablets, or phones containing their data from one location to another since their  data exists on storage media directly attached to their computing or mobile devices. However, in an enterprise setting, moving DAS media required careful planning over weeks or even months. More to come on that.

While this section intends to introduce Storage Architecture, it would be incomplete without highlighting milestones before the Cloud Hybrid Architecture, also commonly referred to as the Information-Centric IT Architecture:

1. **1950s - 1960s: Early Mainframes**
    - Mainframe computers used direct-attached storage (DAS) with local disks connected directly to each mainframe.

2. **1970s - 1980s: Rise of Networked Storage**
    - Network Attached Storage (NAS) emerged, allowing storage devices to be shared across multiple servers via a Local Area Network (LAN).
    - Storage Area Networks (SANs) gained popularity, providing centralized storage accessible by multiple servers.

3. **1990s: SANs and Fibre Channel**
    - Fibre Channel technology enabled high-speed data transfer between servers and storage arrays.
    - SANs became a standard in enterprise data centers.

4. **2000s: Convergence and Virtualization**
    - Converged infrastructure integrated compute, storage, and networking into a single solution.
    - Virtualization technologies (e.g., VMware) abstracted storage from physical hardware.

5. **2010s: Software-Defined Storage (SDS)**
    - SDS solutions allowed storage management through software, decoupling it from specific hardware.
    - Hyperconverged infrastructure (HCI) combined compute and storage in a single appliance.

6. **Present: Cloud and Hybrid Architectures**
    - Cloud providers offer scalable, shared storage services (e.g., Amazon S3, Azure Blob Storage).
    - Hybrid architectures combine on-premises and cloud storage for flexibility.

A quick note: The goal of these videos is to introduce and teach the Storage-Centric IT Architecture, preparing you for a career in Storage Network Engineering or a step further into Cloud Engineering. I will not discuss Cloud Engineering in any depth, but if you're in luck, my partner will teach Cloud in her own installations, and I am sure you all will have a blast with her too.

### Server-Centric Model and DAS Implementation:

![[Server-Centric A.jpg]]

Storage media is directly connected to a server locally, meaning one server at a time can read or write into the directly attached storage media. The application on the server will consume the space on the media to read, write, and execute requests from the client. Some common client types in an enterprise environment are:

- **Type A: End Users**
    - **Role**: Typical employees or consumers who use applications and access data for daily tasks.
    - **Data Access**: Basic access to applications, documents, and shared resources. Examples include office workers, students, and general consumers.
- **Type B: Power Users**
    - **Role**: Users with more advanced needs and higher access privileges, often requiring large data sets for analysis or specialized applications.
    - **Data Access**: Elevated access to data and tools necessary for their work. Examples include data analysts, researchers, and software developers.
- **Type C: Administrative Users**
    - **Role**: IT staff, system administrators, or data managers responsible for maintaining the infrastructure and ensuring data integrity and security.
    - **Data Access**: Full access to all data and system configurations. Examples include IT administrators, network engineers, and database managers.

The connection established between the storage devices and their server is always one server to its storage media. If the server fails, becomes slower, or vulnerable and needs to be updated or replaced, the process of replacing the server becomes a challenge. To improve redundancy, allowing data to be available around the clock, most server-centric IT architectures allow for backup servers to be installed, where two independent servers are directly attached to an array of directly attached storage medias. Should one server need maintenance or fail, the other will pick up the workload using data that is available on the storage media.

### Limitations of the Server-Centric Model and DAS Implementation on a Large Scale:

Just as the internet grew when the demand for more information increased, the demand for more data, space to store the data, and faster processing of data exploded at every enterprise level that serves valuable data. It is certainly possible to continue attaching more disks to a server as the demand increases; however, at some point, the server may run out of resources to process massive loads of data, ultimately becoming slow and the bottleneck in the highway of data traffic. We also run into issues of limited length of Small Computer System Interface (SCSI) cables that serve as the data transfer highway between servers and storage media. These cables can only go so far before they start dropping bits and bytes of data. Additionally, since the relationship of a server to its media is one-to-one, there will always be a break in information transfer whenever the server needs to be maintained, meaning all types of users will experience downtime to access the data they need. Available disk space also goes to waste in this model. If a server is fitted with 500GB and only 200GB of data is consumed, the remaining 300GB will be out of reach to other servers that may need additional space or just a partition of space on the storage media. Hence, this model is both costly and extremely difficult to scale.

Lastly, maintenance is a challenge. The one-to-one relationship of server and storage implies that the same data available on one server may not be  readily available on another server. Managing a physical server and its disk in one area of an IT infrastructure will not trickle down to other similar servers and disks. So, the admin will have to physically coordinate with all the IT administrators in a data-center/enterprise to find times, called maintenance windows, to move through the campus to propagate a change from one server to another. Additionally, since the servers and corresponding disks themselves need to be spread throughout the campus, centralized security management is not possible. Thus, vulnerability to attacks is increased in this architecture.

### Advantages of the Storage-Centric IT Architecture/Model and Network Storage on a Large Scale:

Storage IT Architecture is a network storage mode. 
In contrast to Server-Centric IT Architecture, the Storage-Centric Architecture is a web of interconnected devices typically connected using less constrained high-way cables compared to SCSI cables. It is characterized by one-to-many and many-to-many types of relationships between the server and the storage devices. In this model, the server is an appendage to the storage system that takes the focal point. The dedicated storage media no longer operate in silos, and their space resources are usually more shareable to multiple servers. Lately, disk subsystems, a common type of Storage-Centric model, are usually hyper-converged, meaning the parts that make up the devices are typically coupled together by one main vendor that uses multiple high-quality devices from other vendors to create a stable storage device with large storage capacity in the realm of petabytes. These systems have built-in redundancy features, high scalability, increased security against physical and digital threats, and a centralized management tool, all improving the management of data throughout the enterprise.

![[Storage-Centric A.png]]

### Summary of Key Points
- **The three storage architectures**: Server-Centric, Storage-Centric, and Information-Centric.
- **Data Volume and Infrastructure**: Enterprise data storage needs are often in the petabyte range, requiring sophisticated data centers.
- **Historical Evolution**: From DAS to NAS and SANs, showing the evolution of storage strategies.
- **Challenges of Server-Centric Architecture**: Issues with redundancy, maintenance, scalability, and security.

### Interactive Questions/Quiz
1. What are the three primary architectures for provisioning storage in an enterprise?
2. Why is it not feasible for large companies like Google to use commonly purchasable storage devices?
3. What are the main limitations of the Server-Centric IT Architecture?

### Glossary of Terms
- **DAS (Direct Attached Storage)**
- **NAS (Network Attached Storage)**
- **SAN (Storage Area Network)**
- **SCSI (Small Computer System Interface)**
- **LAN (Local Area Network)**

### Transition to Next Chapter
In the next chapter, we will dive deeper into the Storage-Centric IT Architecture, exploring its components, benefits, and how it addresses the limitations of the Server-Centric model.

### References/Further Reading
- "The Datacenter as a Computer: An Introduction to the Design of Warehouse-Scale Machines" by Luiz André Barroso, Urs Hölzle
- "Storage Networks Explained: Basics and Application of Fibre Channel SAN, NAS, ISCSI, InfiniBand, and FCoE" by Ulf Troppens, Rainer Erkens, Wolfgang Muller-Friedt
- **Flackbox**: An excellent resource for learning about storage and networking technologies, offering in-depth tutorials and guides.

---

### Presenter’s Tips:
1. **Engage with Real-World Examples**: Use relatable analogies and real-world examples, like the Google example, to make complex concepts more understandable.
2. **Maintain a Conversational Tone**: Keep your language conversational to make the presentation feel more natural and engaging.
3. **Pause for Emphasis**: Use pauses to highlight important points and give your audience time to absorb information.
4. **Use Visual Aids Effectively**: Ensure your images and diagrams are clear and directly relevant to the points you're making.
5. **Encourage Interaction**: Pose questions and encourage participation to keep your audience engaged and to check their understanding.

Feel free to make any additional adjustments or let me know if there are other areas you'd like to focus on!