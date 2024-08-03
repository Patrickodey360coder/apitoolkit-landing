---
title: Pricing
date: 2022-03-23
updatedDate: 2024-06-15
faqs:
  - q: What programming languages are supported?
    a: We currently support 17+ web frameworks in different programming languages (Python, Golang, Javascript, PHP, C#, Java, etc.). If we don't support your framework, kindly email us at <a href="mailto:hello@apitoolkit.io">hello@apitoolkit.io</a> and we'll create an SDK for you ASAP!
  - q: Do my requests have to leave my server to APItoolkit servers?
    a: If you want to benefit from the API monitoring and the log explorer feature, yes. However, we offer an <a href="/pricing/">enterprise plan</a> that allows you to run APItoolkit on-prem (on your servers).
  - q: Will your SDKs slow down my backend?
    a: It depends. Most SDKs stream data asynchronously via Google PubSub, so your requests will see almost zero change in performance. However, if you use PHP you may pay a very tiny performance hit to send data to Google PubSub. This is because PHP doesn't support async workflows by default. But if you have the GRPC extension installed in your PHP environment, it will be used by PubSub to stream data asynchronously like in other languages. But this performance hit is barely noticeable and usually under 5ms added to every request.
  - q: How do you handle security and sensitive data?
    a: We take security seriously at APItoolkit. We employ encryption and authentication measures to ensure the security of your data during transmission and storage. All our SDKs also support redacting data. You can simply specify the JSONPath to the fields that you don't want the SDKs to forward to APItoolkit, and those sensitive fields will be stripped out/redacted before the data even leaves your servers and replaced with the text "[CLIENT REDACTED]" on our end. We will never see anything you don't want us to see.
  - q: I really love what you're doing. How can I show support?
    a: Give us a shout-out on X (Twitter), Discord, or any social media you use. We would also appreciate honest feedback about what we're building and suggestions for what functionality you would love to see next.
---

```=html
<div class="w-full">
    <header class="w-full mt-32">
        <div class="width-control mx-auto px-2">
            <div class="w-full flex flex-col items-center text-center gap-4">
                <h1 class="font-bold text-4xl text-[50px] dark:text-white">Pay Only for What You Use</h1>
                <p class="max-w-[300px] md:max-w-[500px] text-base-content font-medium text-base md:text-lg">Trust your APIs and only pay for the requests we analyze.</p>
            </div>
        </div>
    </header>
    <div class="width-control mx-auto px-2">
        <section class="w-full grid md:grid-cols-2 lg:grid-cols-3 gap-8 py-24">
            <!-- FREE PLAN -->
            <a href="https://app.apitoolkit.io/p/new"
                class="group rounded-2xl border p-6 flex duration-300 flex-col justify-start gap-6 text-left bg-base-100 shadow-md hover:-translate-y-3">
                <div class="flex flex-col gap-3 pb-1">
                    <h3 class="font-medium text-3xl">Free</h3>
                    <div>
                        <div class="flex flex-row items-start gap-8px">
                            <div class="font-bold text-6xl">
                               $0 <small class="text-sm">per month.</small>
                            </div>
                        </div>
                        <div class="text-sm leading-120 font-medium text-base-content">
                            20,000 requests/month included for free.
                        </div>
                    </div>
                </div>
                <div class="text-base-content">
                    <p class="font-bold mb-3">For Developers</p>
                    <ul class="flex flex-col gap-3 text-sm font-medium">
                        <li class="flex flex-row gap-2">
                            <div
                                class="inline-block text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>2 team members
                        </li>
                        <li class="flex flex-row gap-2">
                            <div
                                class="inline-block text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>All free features
                        </li>
                        <li class="flex flex-row gap-2">
                            <div
                                class="inline-block text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            Last 7 days data retention
                        </li>
                    </ul>
                </div>
                <button as="button" class="mt-auto rounded-xl flex flex-row justify-center px-4 py-[7px] border"><span class="z-1 relative">Get Started</span></button>
            </a>
            <!-- PAY AS YOU USE PLAN -->
            <!-- <a href="https://app.apitoolkit.io/p/new"
                class="group rounded-2xl border p-6 flex duration-300 flex-col text-gray-50 justify-start gap-6 text-left bg-blue-600 shadow-md hover:-translate-y-3">
                <div class="flex flex-col gap-3 pb-1">
                    <h3 class="font-medium text-3xl">Pay as You Use</h3>
                    <div>
                        <div class="flex flex-row items-start gap-8px">
                            <div class="font-bold text-6xl">
                                $1 <small class="text-sm">per 10,000 requests.</small>
                            </div>
                        </div>
                        <div class="text-sm leading-120 font-medium text-gray-200">
                            Get 40% off by pre-paying for up to a year.
                        </div>
                    </div>
                </div>
                <div class="text-gray-50">
                    <p class="font-bold mb-3">For Small Teams</p>
                    <ul class="flex flex-col gap-3 text-sm font-medium">
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>Unlimited team members
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API testing pipelines
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API swagger/OpenAPI hosting
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API metrics custom monitors
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API live traffic AI-based validations
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            Last 14 days data retention
                        </li>
                    </ul>
                </div>
                <button as="button"
                    class="mt-auto rounded-xl text-white flex flex-row justify-center px-4 py-[7px] border">
                    Get Started
                </button>
            </a>
            GRADUATED PLAN -->
            <div
                class="group rounded-2xl border p-6 flex duration-300 flex-col text-gray-50 justify-start gap-6 text-left bg-blue-600 shadow-md">
                <div class="flex flex-col gap-3 pb-1">
                <div class="flex justify-between items-center">
                 <h3 class="font-medium text-[25px]">Pay as You Use</h3>
                <div role="tablist" class="tabs tabs-boxed tabs-xs">
                   <input onchange="handlePlanToggle()" value="month" type="radio" name="plans" role="tab" class="tab" aria-label="Monthly" checked>
                   <input onchange="handlePlanToggle()" value="annual" type="radio" name="plans" role="tab" class="tab" aria-label="Annual">
                </div>
                </div>
                    <div>
                        <div class="flex flex-col items-start gap-8px">
                            <span class="text-sm font-bold text-yellow-200" id="starts_at">Starts at ...</span>
                            <div class="">
                               <span class="font-bold text-6xl" id="price">$19</span>
                               <span class="" id="num_requests">/400k requests</span>
                               <br>
                               <small class="text-sm">then $1 per 20k requests</small>
                               <span id="save_container" class="text-yellow-200 font-bold"></span>
                            </div>
                        </div>
                    </div>
                </div>
                <div>
                  <input type="range" min="0" max="6" step="1" value="0" class="range range-primary range-sm" id="price_range">
                </div>
                <div class="text-gray-50">
                    <p class="font-bold mb-3">For Growing Companies</p>
                    <ul class="flex flex-col gap-3 text-sm font-medium">
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>Unlimited team members
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API testing pipelines
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API swagger/OpenAPI hosting
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API metrics custom monitors
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            API live traffic AI-based validations
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-200 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            Last 14 days data retention
                        </li>
                    </ul>
                </div>
                <a  href="https://app.apitoolkit.io/p/new"
                    class="mt-auto rounded-xl text-white flex flex-row justify-center px-4 py-[7px] border">
                    Get Started
                </a>
            </div>
            <!-- ENTERPRISE PLAN -->
            <a href="mailto:hello@apitoolkit.io"
                class="group rounded-2xl border p-6 flex duration-300 flex-col text-gray-50 justify-start gap-6 text-left bg-slate-900 shadow-md hover:-translate-y-3">
                <div class="flex flex-col gap-3 pb-1">
                    <h3 class="font-medium text-3xl">Enterprise</h3>
                    <div>
                        <div class="flex flex-row items-start gap-8px">
                            <div class="font-bold text-6xl">Contact us
                            </div>
                        </div>
                        <div class="text-sm leading-120 font-medium text-gray-200">
                            Configure high-volume custom amounts based on demand.
                        </div>
                    </div>
                </div>
                <div class="text-gray-50">
                    <p class="font-bold mb-3">For Large Organizations</p>
                    <ul class="flex flex-col gap-3 text-sm font-medium">
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-500 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>Custom team members
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-500 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            High volume discounts
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-500 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            On-prem or on your own infra
                        </li>
                        <li class="flex flex-row gap-2">
                            <div class="text-center font-bold bg-gray-500 text-green-500 rounded-md w-5 h-5">
                                ✓
                            </div>
                            Custom data retention
                        </li>
                    </ul>
                </div>
                <button as="button" class="mt-auto rounded-xl flex flex-row justify-center px-4 py-[7px] border">Contact Sales</button>
            </a>
        </section>

        <section class="w-full py-4">
        {% render "default/components/customers" %}

        <hr />

        {% render "default/components/faqs", this:this %}
        </section>

    </div>
</div>
```
