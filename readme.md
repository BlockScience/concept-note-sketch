# Discrete Event Dynamical System with Components and Templates

This repository contains a Python implementation of a discrete event dynamical system using components, concepts, and templates. The system is designed to create and manage documents based on these components and templates, ensuring compatibility and facilitating revisions.

## Mathematical Description

### State Space

The state space $\mathcal{X}$ consists of states $X$, each defined by a collection of concepts $\vec{x} \in X$. A state $X$ is a snapshot of the system at a given time, capturing the structure and organization of its nested components.

- **Concepts**: Each concept $\vec{x}$ is represented as a collection (or vector) of components $x_i$:
  $$\vec{x} = (x_1, x_2, \ldots, x_n)$$
  - **Components**: Each component $x_i$ has content, a version, and a type. It is defined by the following structure:
    $$x_i = (\text{content}_i, \text{version}_i, \text{type}_i)$$
    - **Component Type**: A component type includes a name and requirements:
      $$\text{type}_i = (\text{name}_i, \text{requirements}_i)$$

Thus, a state $X$ in the state space $\mathcal{X}$ is a collection of these concepts, each containing a structured collection of components.

### State Update Rules

The state $X$ is updated through revisions or additions of components within the concepts $\vec{x} \in X$. Let $u$ be an input or action that triggers the state update. The state update rules describe how the system evolves over time:

1. **Component Revision**: Each component $x_i$ can be revised, resulting in a new component $x_i^+$ and an updated concept $\vec{x}^+$:
   $$ x_i^+ = (\text{new\_content}_i, \text{version}_i + 1, \text{type}_i)$$
   $$\vec{x}^+ = (x_1, \ldots, x_i^+, \ldots, x_n)$$

2. **Component Addition**: New components can be added to a concept, extending the vector:
   $$\vec{x}' = (x_1, x_2, \ldots, x_n, x_{n+1})$$

The overall state update rule is defined as:
$$X^+ = f(X, u)$$

### Measurable Outputs

Documents are considered measurable outputs $y$ and are constructed from the state $X$ according to specific templates:
$$y = h(X)$$

Templates define the required pattern of component types for a document. The function $\mathcal{T}(\vec{x})$ checks if a concept $\vec{x}$ is compatible with a template $\mathcal{T}$:

$$\mathcal{T}(\vec{x}) = 
\begin{cases} 
1 & \text{if } \vec{x} \text{ is compatible with } \mathcal{T} \\
0 & \text{otherwise}
\end{cases}$$

The `document_factory` function creates a document from a concept $\vec{x}$ and a template $\mathcal{T}$, given that $\vec{x}$ is compatible with $\mathcal{T}$:

$$\text{document_factory}(\vec{x}, \mathcal{T}) = 
\begin{cases}
\text{Document} & \text{if } \mathcal{T}(\vec{x}) = 1 \\ 
\text{Error} & \text{if } \mathcal{T (\vec{x}) = 0 
\end{cases}$$

## Code Overview

### Classes

- **Component_Type**: Defines a type of component with a name and requirements.
- **Component**: Represents a component with content, version, and type. Includes a method to revise the component.
- **Concept**: Represents a concept with a name, version, description, and a list of components. Includes methods to add and revise components.
- **Template**: Defines a template with a name, description, and a pattern of required component types. Includes a method to check if a concept matches the template.
- **Document**: Represents a document with a name, version, components, concept, and template. Includes a method to export the document content.

### Functions

- **document_factory**: Creates a document from a concept and a template, ensuring the concept is compatible with the template.

## Examples

### Example 1: Creating and Revising a Two-Pager Document

1. **Define Component Types and Templates**:

```python
legalese = Component_Type("Legalese", "Legal requirements")
summary = Component_Type("Summary", "Summary of the concept")
quadchart = Component_Type("Quadchart", "Quadchart of the concept")
approach = Component_Type("Approach", "Approach to the concept")
feasibilityRisksAssumptions = Component_Type("Feasibility, Risks, Assumptions", "Description of the feasibility, risks, and assumptions of the concept")
successCriteriaMilestones = Component_Type("Success Criteria and Milestones", "Success criteria and milestones of the concept")

TwoPager = Template("Two-Pager", "A 2-page document", [legalese, summary, quadchart])
SixPager = Template("Six-Pager", "A 6-page document", [legalese, summary, quadchart, approach, feasibilityRisksAssumptions, successCriteriaMilestones])
```

2. **Create Sample Components**:

```python
legalese_component = Component("Legalese content Lipsum etc", 1, legalese)
summary_component = Component("Summary content Lipsum etc", 1, summary)
quadchart_component = Component("Quadchart content Lipsum etc", 1, quadchart)
approach_component = Component("Approach content Lipsum etc", 1, approach)
feasibilityRisksAssumptions_component = Component("Feasibility, Risks, Assumptions content Lipsum etc", 1, feasibilityRisksAssumptions)
successCriteriaMilestones_component = Component("Success Criteria and Milestones content Lipsum etc", 1, successCriteriaMilestones)
```

3. **Define a Concept**:

```python
my_concept = Concept("Concept 1", 1, "Description of concept 1", [legalese_component, summary_component, quadchart_component, approach_component])
```

4. **Check Compatibility and Create a Two-Pager Document**:

```python
TwoPager.check_concept(my_concept)
twopagedocument = document_factory(my_concept, TwoPager)
print(twopagedocument.export())
```

5. **Revise a Component and Create a New Document**:

```python
my_concept.revise_component(summary_component, "New summary Lipsum etc")
new_twopagedocument = document_factory(my_concept, TwoPager)
print(new_twopagedocument.export())
```

### Example 2: Creating a Six-Pager Document

1. **Add Missing Components**:

```python
feasibilityRisksAssumptions_component = Component("Feasibility, Risks, Assumptions content Lipsum etc", 1, feasibilityRisksAssumptions)
my_concept.add_component(feasibilityRisksAssumptions_component)

successCriteriaMilestones_component = Component("Success Criteria and Milestones content Lipsum etc", 1, successCriteriaMilestones)
my_concept.add_component(successCriteriaMilestones_component)
```

2. **Check Compatibility and Create a Six-Pager Document**:

```python
SixPager.check_concept(my_concept)
sixpage_document = document_factory(my_concept, SixPager)
print(sixpage_document.export())
```

3. **Revise a Component and Create a New Document**:

```python
my_concept.revise_component(summary_component, "New NEW summary Lipsum etc")
newnew_twopagedocument = document_factory(my_concept, TwoPager)
print(newnew_twopagedocument.export())
```

4. **Create a New Concept and a Six-Pager Document**:

```python
new_legalese_component = Component("New Legalese content Lipsum etc", 1, legalese)
new_summary_component = Component("New Summary content Lipsum etc", 1, summary)
new_quadchart_component = Component("New Quadchart content Lipsum etc", 1, quadchart)
new_approach_component = Component("New Approach content Lipsum etc", 1, approach)
new_feasibilityRisksAssumptions_component = Component("New Feasibility, Risks, Assumptions content Lipsum etc", 1, feasibilityRisksAssumptions)
new_successCriteriaMilestones_component = Component("New Success Criteria and Milestones content Lipsum etc", 1, successCriteriaMilestones)

new_concept = Concept("Concept 2", 1, "Description of concept 2", [new_legalese_component, new_summary_component, new_quadchart_component, new_approach_component, new_feasibilityRisksAssumptions_component, new_successCriteriaMilestones_component])

new_sixpage_document = document_factory(new_concept, SixPager)
print(new_sixpage_document.export())
```

## Conclusion

This repository demonstrates a discrete event dynamical system using Python. It involves defining components, concepts, and templates to create and manage documents. The provided examples illustrate how to
