## SDK in polling mode vs SDK in streaming mode

LaunchDarkly offers two modes of feature flag delivery for its SDKs: **polling mode** and **streaming mode**. Both have different use cases based on your application requirements and infrastructure constraints. Here’s a breakdown:

### 1. **Streaming Mode (Default mode)**

In **streaming mode**, the SDK establishes a persistent connection to LaunchDarkly’s servers using **server-sent events (SSE)**. When a flag's configuration changes, LaunchDarkly immediately pushes the update through the open connection to the SDK. This mode ensures that your application always has the most up-to-date feature flags with minimal latency.

- **How it works:**
  - The SDK opens a streaming connection.
  - Flag updates are pushed in real-time from LaunchDarkly to your application.

- **Advantages:**
  - **Real-time updates**: You get instant updates when feature flags change.
  - **Efficient**: Fewer API calls are made, since changes are pushed rather than polled.
  - **Lower Latency**: Flag changes are applied with minimal delay.

- **When to use Streaming Mode:**
  - For **low-latency** use cases where flag changes should be applied immediately.
  - In environments where **long-lived connections** are allowed, such as backend services, microservices, or server-based applications.
  - If your application is **high traffic** or **mission-critical**, and it needs to reflect flag changes as soon as they happen.

### 2. **Polling Mode**

In **polling mode**, the SDK periodically polls (makes HTTP requests) LaunchDarkly’s servers to check for any changes to feature flags. If there are changes, they are pulled by the SDK during the next polling interval.

- **How it works:**
  - The SDK regularly queries the LaunchDarkly API (e.g., every 30 seconds).
  - The SDK checks if there are any flag updates, and applies them if needed.

- **Advantages:**
  - **Simplicity**: No need to maintain a persistent connection.
  - **Firewall-friendly**: Polling works well in environments where **long-lived connections are not possible** or discouraged, such as mobile devices, edge devices, or certain corporate networks.
  
- **Disadvantages:**
  - **Slower updates**: Flags are only updated during the next polling interval (depending on your configured interval, this could be seconds to minutes).
  - **More API calls**: Higher API usage compared to streaming mode, as the SDK makes periodic HTTP requests.

- **When to use Polling Mode:**
  - In environments where **persistent connections are not feasible** or allowed, such as mobile applications or devices behind corporate firewalls that block SSE or WebSocket connections.
  - In **resource-constrained environments**, where maintaining an open connection is too costly in terms of battery, bandwidth, or network conditions.
  - When **immediate updates** are not as critical, and you can tolerate small delays between flag changes and application behavior changes.

### When to choose one over the other:
- **Use Streaming Mode** if you need **real-time** flag updates with low latency, especially in server environments.
- **Use Polling Mode** when you are in **network-restricted** environments (such as mobile apps or certain corporate networks) where persistent streaming connections aren’t possible or where real-time updates aren't essential.
