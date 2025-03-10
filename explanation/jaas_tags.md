# JAAS Tags

## Introduction

When dealing with Relation Based Access Control (**ReBAC**) in JAAS we
use **JAAS tags** when referring to resources.

Each **JAAS tag** uniquely identifies a resource.

## Users

A user tag has the following format:

```text
user-<username>
```

where `username` uniquely identifies a user including the domain.

## Group

A group tag has the following format:

```text
group-<group id>
```

where `group id` represents the unique identifier of the group.

## Role

A role tag has the following format:

```text
role-<role name>
role-<role id>
```

where `role id` represents the unique identifier of the role.

## Controller

A controller tag has the following format:

```text
controller-<controller name>
```

## Cloud

A cloud tag has the following format:

```text
cloud-<cloud name>
```

## Model

A model tag has the following format:

```text
model-<controller name>/<model name>
```

where `controller name` specifies name of the controller on which the model
is running and `model name` specifies the name of the model.

## Application Offer

An application offer tag has the following format:

```text
applicationoffer-<controller name>/<model name>.<offer name>
```

where `controller name` specifies name of the controller on which the model
is running, `model name` specifies name of the model in which the application
offer was created and `offer name` specifies the name of the application offer.