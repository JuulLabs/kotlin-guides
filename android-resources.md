# Naming Conventions

## Why?

Per [Joroean Mol][joroenmol]: 

> Whenever we start a new project, we take a lot of care in setting up our architecture, CI, build flavors,… But do you also have a strategy to name your resources?
>
> You should! Because the lack of XML namespaces, makes managing Android resources tedious. And causes things to grow out of control easily, especially in large projects.
>
> So let’s introduce a simple scheme that will solve your pains.
>
> - easy lookup of any resource (autocomplete)
> - logical, predictable names
> - clean ordering of resources
> - strongly typed resources

I propose that we do not adopt his final mechanism, but and instead rely on an older tool for information gathering.

## Basic Principle

A global, hierarchical naming structure based on the [Five Ws][fivews], with some extras:

1. Subtype
2. Who
3. What
4. Where
5. When
6. Why
7. Value

## Element Types

### Subtype
A subtype for a resource.
Generally, these should be deterministic 

- Drawables should use one of the following:
  - `ic` for icons, as is standard Android convention.
  - `grad` for gradients
  - `pic` for pictures or illustrations
  - `shape` for shapes
  - `layers` for layer lists
  - `selector` for selectors
- Ids for a view should use a (snake case) version of the view's type, such as `button` or `text_view`.
- Layout will always be one of the following: 
  - `activity`, `fragment`, and `include` should all be self-explanatory
  - `item` for adapter items like recyclers/view pagers
  - `view` for custom views

### Who
The owner of a resource.
For resources that do not have a clear owner or are shared between unrelated owners, use `all`.

### What
The description of a resource.
This should be kept broad whenever possible to group things together.
If further detail is necessary, it can be added here.
For the sake of making things explicit, this is marked as another column on the example table.

### When
The condition of a resource.
Use it for things like selectors/state lists, an `include` layout which is only sometimes visible, or associating an error's cause to its message.

### Where
The location of a resource, such as where it appears in a layout if it occurs more than once (a link in both the `body` and `footer`, perhaps).

### Why
An motivation for a resource, such as legacy version support (`v21`).

### Value
A raw value associated with the resource, that is necessary to identify it.
Example uses are alpha-blended colors (specify the opacity percent).

### Crafting Names
Elements should be appended in the order above, only including ones that are actually useful.
If an element contains multiple words, each word must be separated with an underscore (`_`).
Elements must also be separated by an underscore.

Resource types where `all` is more-or-less universal (like colors, ids, drawables) may skip listing it.

### Examples

| Resource Type | Subtype    | Who              | What         | (detail)   | Where | When            | Why | Value | Combined Name                        |
| ------------- | ---------- | ---------------- | ------------ | ---------- | ----- | --------------- | --- | ----- | ------------------------------------ |
| **color**     |            |                  | `black`      | `alpha`    |       |                 |     | `50`  | `black_alpha_50`                     |
| **color**     |            |                  | `mint_green` |            |       |                 |     |       | `mint_green`                         |
| **dimen**     |            | `all`            | `margin`     |            |       |                 |     |       | `all_margin`                         |
| **dimen**     |            | `grid`           | `10`         |            |       |                 |     |       | `grid_10`                            |
| **drawable**  | `ic`       |                  | `chevron`    | `right`    |       |                 |     |       | `ic_chevron_right`                   |
| **id**        | `button`   |                  | `sign_out`   |            |       |                 |     |       | `button_sign_out`                    |
| **layout**    | `activity` | `main`           |              |            |       |                 |     |       | `activity_main`                      |
| **layout**    | `include`  | `av`             | `banner`     |            |       | `no_internet`   |     |       | `include_av_banner_no_internet`      |
| **string**    |            | `all`            | `action`     | `sign_out` |       |                 |     |       | `all_action_sign_out`                |
| **string**    |            | `application`    | `title`      |            |       |                 |     |       | `application_title`                  |
| **string**    |            | `authentication` | `error`      |            |       | `invalid_email` |     |       | `authentication_error_invalid_email` |

## Alternatives

- Joroen Mol's ["A successful XML naming convention"][joroenmol]
- MindOrk's ["Android Resource Naming Convention"][mindork]
- Many others! This is a popular blog topic

[fivews]: https://en.wikipedia.org/wiki/Five_Ws
[joroenmol]: https://jeroenmols.com/blog/2016/03/07/resourcenaming/
[mindork]: https://medium.com/mindorks/android-resource-naming-convention-42e4e8026614
