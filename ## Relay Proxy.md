You need to manage your own proxy service, while the Relay Proxy adds great benefits like caching and reducing network traffic

The **LaunchDarkly Relay Proxy** is an optional component that acts as an intermediary between the LaunchDarkly servers and your SDK clients. Its primary purpose is to improve performance, reduce network traffic, and provide more control over flag delivery, especially in larger or more complex infrastructures. 

Here's a detailed explanation of what the Relay Proxy is, how it works, and when to use it:

### What is the LaunchDarkly Relay Proxy?

The **Relay Proxy** serves as a local cache for feature flag configurations and flag evaluation data. Instead of every SDK client (such as your microservices, applications, or other environments) individually connecting to LaunchDarkly's servers, they can connect to the **Relay Proxy**. The Relay Proxy manages the connection with LaunchDarkly, keeps a cache of the flag data, and distributes this data to the SDK clients.

### How Does the Relay Proxy Work?

1. **Connection to LaunchDarkly**: The Relay Proxy establishes a connection to LaunchDarkly’s servers in **streaming mode** (using server-sent events or SSE). It pulls down the latest flag values from LaunchDarkly.
   
2. **Caching of Flag Data**: Once it has received the flag data, the Relay Proxy caches it locally. This allows the SDK clients that connect to the Relay Proxy to retrieve flag configurations without needing to make separate requests to LaunchDarkly’s servers.

3. **SDK Client Connections**: SDK clients (in your application) connect to the Relay Proxy instead of LaunchDarkly directly. The SDKs retrieve the cached flag values from the proxy, reducing latency and minimizing external network calls.

4. **Flag Updates**: When there is a change to a flag, LaunchDarkly pushes the change to the Relay Proxy, which updates its cache and immediately makes the new flag values available to all connected SDK clients.

5. **Flag Evaluation**: Flag evaluation (deciding whether a feature should be on or off for a particular user or segment) still happens locally in the SDK client, but the client gets the necessary data (flag configuration, targeting rules, etc.) from the Relay Proxy instead of directly from LaunchDarkly.

### Key Benefits of Using the Relay Proxy

1. **Improved Performance**: The Relay Proxy helps reduce latency in feature flag delivery by caching flag configurations locally. SDK clients no longer need to connect to LaunchDarkly servers, which can reduce the time it takes to serve feature flag variations.
   
2. **Reduced Network Traffic**: Without the Relay Proxy, each SDK instance would need to maintain its own connection to LaunchDarkly. In large-scale applications with many instances (e.g., microservices or containerized environments), this can lead to a significant amount of network traffic. The Relay Proxy consolidates these connections, reducing the load on your network and LaunchDarkly’s servers.

3. **Better Control in Complex Environments**:
   - **On-Premise or Edge Deployments**: For applications running in environments with limited or restricted access to external services (e.g., on-premise, edge devices, or certain network zones), the Relay Proxy can serve as a controlled point of access to LaunchDarkly. This allows SDK clients to access feature flags even when direct internet access is restricted.
   - **Network Segmentation**: You can deploy the Relay Proxy inside your internal network, allowing all SDKs within that network to connect to it instead of making requests over the public internet.

4. **High Availability**: If your environment has multiple instances of the Relay Proxy, they can act as redundancy for each other, ensuring continuous delivery of flag data even if one Relay Proxy goes down.

5. **Centralized Access Point for Feature Flags**: In complex infrastructures, especially when running multiple services or microservices, the Relay Proxy can act as a single point of access for feature flags, simplifying management and scaling.

### When to Use the Relay Proxy

1. **Large-Scale Distributed Applications**: If your application architecture includes many services, microservices, or instances (e.g., thousands of servers, containers, or edge devices), the Relay Proxy helps reduce the overhead of having each instance connect directly to LaunchDarkly.

2. **Low-Latency or High-Throughput Requirements**: In performance-sensitive environments where low-latency flag evaluations are crucial (e.g., gaming, financial services, real-time data processing), caching flag data locally via the Relay Proxy can reduce response times.

3. **Restricted or Isolated Networks**: If your environment has limited access to the public internet or needs to operate in an isolated network (e.g., on-premises environments or government/military deployments), the Relay Proxy can ensure your SDK clients still get feature flag data without needing direct access to LaunchDarkly's cloud servers.

4. **Regional or Multi-Zone Deployments**: In globally distributed applications where different regions or zones need quick access to flag data, you can deploy multiple Relay Proxy instances in various regions to reduce latency and increase fault tolerance.

5. **Cloud or Containerized Environments**: If you’re deploying in containerized environments like Kubernetes or running many cloud-based instances, the Relay Proxy can help manage connections to LaunchDarkly, providing a more efficient way to distribute feature flags across dynamic, ephemeral environments.

### Example Setup:

- You have a microservices-based architecture with hundreds of services running across different regions.
- Instead of each service connecting directly to LaunchDarkly’s servers, you deploy the Relay Proxy in each region.
- Each service connects to the local Relay Proxy, reducing latency and network traffic.

### Summary of Usage:
- **Improved flag delivery performance** through local caching.
- **Reduced external network traffic** by consolidating SDK connections.
- **Better control over access** in restricted or isolated environments.
- **Scaling** to large infrastructures or globally distributed applications.

The Relay Proxy is most beneficial for organizations with large-scale, high-traffic applications or complex infrastructure that needs efficient feature flag management with high performance and reliability.