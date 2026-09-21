# Topic 01: Containers 101

Before containers, code that worked on one machine often broke on another, because of different library versions or missing dependencies. A **container** packages an app with everything it needs to run, and shares the host machine's kernel instead of needing its own — which is why containers start in under a second and use megabytes, not gigabytes, unlike a full virtual machine.

An **image** is a read-only template. A **container** is a running instance of that template — you can create many independent containers from one image, and none of them affect each other or the image itself.

**How this topic is organized:** This topic is entirely local — there's no separate 'AWS version' of learning what a container is. You'll build on this understanding starting in Topic 2. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
