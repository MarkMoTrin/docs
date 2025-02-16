---
title: "Endpoint configurations"
sidebar_position: 1
---

The following are configurable settings within an Endpoint.

## Endpoint Name

Create a name you'd like to use for the Endpoint configuration.
The resulting Endpoint is assigned a random ID to be used for making calls.

The name is only visible to you.

## GPU Selection

Select one or more GPUs that you want your Endpoint to run on. RunPod matches you with GPUs in the order that you select them, so the first GPU type that you select is prioritized, then the second, and so on. Selecting multiple GPU types can help you get a worker more quickly, especially if your first selection is an in-demand GPU.

## Active (Min) Workers

Setting this amount to 1 will result in "always on" workers.
This will allow you to have a worker ready to respond to job requests without incurring any cold start delay.

:::note

You will incur the cost of any active workers you have set regardless if they are working on a job.

:::

## Max Workers

Set an upper limit on the number of active workers your endpoint has running at any given point. Setting a value for max workers that is too low can lead to [throttled workers](/glossary#throttled-worker). If you consistently see throttled workers, increase your max workers to five or more.

Default: 3

<details>
<summary>

How to configure Max Workers

</summary>
You can also configure a max worker count. This is the top limit of what RunPod will attempt to auto-scale for you. Use this to cap your concurrent request count and also limit your cost ceiling.

:::note
We currently base your caching coefficient by this number, so an endpoint with higher max worker count will also receive a higher priority when caching workers.

This is partially why we limit new accounts to a relatively low max concurrency at the account level. If you want to get this number raised, you generally will need to have a higher history of spending, or commit to a relatively high spend per month.

You should generally aim to set your max worker count to be 20% higher than you expect your max concurrency to be.

:::

</details>

## GPUs / Worker

The number of GPUs you would like assigned to your worker.

:::note

Currently only available for 48GB GPUs.

:::

## Idle Timeout

The amount of time in seconds a worker not currently processing a job will remain active until it is put back into standby.
During the idle period, your worker is considered running and will incur a charge.

Default: 5 seconds

## FlashBoot

RunPod magic to further reduce the average cold-start time of your endpoint.
FlashBoot works best when an endpoint receives consistent utilization.
There is no additional cost associated with FlashBoot.

## Advanced

Additional controls to help you control where your endpoint is deployed and how it responds to incoming requests.

### Data Centers

Control which data centers can deploy and cache your workers. Allowing multiple data centers can help you get a worker more quickly.

Default: all data centers

### Select Network Volume

Attach a network storage volume to your deployed workers.

Network volumes will be mounted to `/runpod-volume/`.

:::note

While this is a high performance network drive, do keep in mind that it will have higher latency than a local drive.

This will limit the availability of cards, as your endpoint workers will be locked to the datacenter that houses your network volume.

:::

### Scale Type

- **Queue Delay** scaling strategy adjusts worker numbers based on request wait times. With zero workers initially, the first request adds one worker. Subsequent requests add workers only after waiting in the queue for the defined number of delay seconds.
- **Request Count** scaling strategy adjusts worker numbers according to total requests in the queue and in progress. It automatically adds workers as the number of requests increases, ensuring tasks are handled efficiently.

```text
_Total Workers Formula: Math.ceil((requestsInQueue + requestsInProgress) / <set request count)_
```

### GPU Types

Within the select GPU size category you can further select which GPU models you would like your endpoint workers to run on.
Default: `4090` | `A4000` | `A4500`

<details>
<summary>
What's the difference between GPU models.
</summary>
A100s are about 2-3x faster than A5000s and also allow double the VRAM with very high bandwidth throughout. 3090s and A5000s are 1.5-2x faster than A4000s. Sometimes, it may make more sense to use 24 GB even if you don't need it compared to 16 GB due to faster response times. Depending on the nature of the task, it's also possible that execution speeds may be bottlenecked and not significantly improved simply by using a higher-end card. Do your own calculations and experimentation to determine out what's most cost-effective for your workload and task type.

Want access to different flavors? [Let us know](https://www.runpod.io/contact) and we can look at expanding our offerings!

</details>

## CUDA version selection

You have the ability to select the allowed CUDA versions for your workloads.
The CUDA version selection determines the compatible GPU types that will be used to execute your serverless tasks.

Specifically, the CUDA version selection works as follows:

- You can choose one or more CUDA versions that your workload is compatible with or requires.
- RunPod will then match your workload to available GPU instances that have the selected CUDA versions installed.
- This ensures that your serverless tasks run on GPU hardware that meets the CUDA version requirements.

For example, if you select CUDA 11.6, your serverless tasks will be scheduled to run on GPU instances that have CUDA 11.6 or a compatible version installed. This allows you to target specific CUDA versions based on your workload's dependencies or performance requirements.
