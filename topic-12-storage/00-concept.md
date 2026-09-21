# Topic 12: Storage

A PersistentVolume (PV) is a piece of storage in the cluster. A PersistentVolumeClaim (PVC) is a REQUEST for storage by a Pod - the cluster-scale version of the Docker named volumes from Topic 3. On kind, storage is backed by your local disk and disappears with the cluster. On EKS, a PVC provisions a REAL AWS EBS volume, which has a real, ongoing cost until deleted - this is one of the few things in this course that keeps costing money even when nothing is actively running.

**How this topic is organized:** Local exercises: PV/PVC concept and proving data survives Pod deletion, entirely free on kind. AWS exercise: the same pattern backed by a real EBS volume, with extra-careful cleanup since EBS volumes bill by the GB-month regardless of whether anything is using them. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
