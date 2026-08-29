# WhiteStarUML `.uml` / XPD Generation Rules

Use this reference only when creating or directly modifying a WhiteStarUML file.

## Optional Generic Structural Reference

`whitestar-xpd-structural-reference.uml`, stored in this same `references` folder, is a generic XPD example with placeholder class names and no project business content. It is optional: generation and validation must use the self-contained contract below, not depend on this file.

Use it only as an emergency aid for understanding XPD nesting or field placement. Never reuse its GUIDs, model/package ownership, class names, operations, attributes, or diagram content in a generated deliverable.

## Safe Workflow

1. Confirm WhiteStarUML has closed the target file before writing.
2. Preserve the original with a backup or produce a separately named output.
3. Preserve the target file's root XPD namespace and profile exactly. Do not mix XPD families or profiles merely because the XML would parse.
4. Parse the document and construct every relationship from the self-contained XPD contract in this reference. Do not require a separate “known-good” `.uml` file in order to generate or validate a diagram.
5. Generate globally unique GUIDs for every new model and view object.
6. Add both semantic model elements and presentation/view elements. A visible line is not a substitute for a model relationship, and a model relationship without a valid view may not appear.
7. Update all owning collections and reverse-reference collections.
8. Recalculate every affected count attribute.
9. Validate XML parsing, GUID uniqueness, reference resolution, collection counts, and diagram ownership before delivery.

## Sequence-Diagram Colours

Unless the user explicitly requests another colour scheme, the required WhiteStarUML sequence-diagram style is deep-red lines with black text. Set the following values explicitly for every new sequence element:

- `UMLSeqStimulusView` message arrow: `LineColor` = `clMaroon`;
- its `NameLabel` message text: `FontColor` = `clBlack`;
- `UMLSeqObjectView` object frame: `LineColor` = `clMaroon`;
- its `NameLabel` and `StereotypeLabel` text: `FontColor` = `clBlack`;
- its `UMLLifeLineView` lifeline: `LineColor` = `clMaroon`.

Do not set `LineColor = clBlue`, `FontColor = clBlue`, or `FontColor = clMaroon` on newly generated sequence elements under this default style. A different colour is permitted only when the user expressly requests it.

After editing, inspect every new `UMLSeqStimulusView`, its `NameLabel`, and every new object view. Verify the explicit values `clMaroon` for lines and `clBlack` for labels before delivery. Do not infer colours from default application rendering.

## Class-Diagram Colour

Unless the user explicitly requests another style, every `UMLClassView` in a generated or edited WhiteStarUML class diagram must use the same pale-yellow frame fill:

- `FillColor` = `$00B9FFFF` (RGB `#FFFFB9`).

Apply this to **all** class boxes in each affected class diagram, irrespective of stereotype (`<<boundary>>`, `<<control>>`, `<<entity>>`, DAO, interface implementation, and so on). Stereotypes distinguish responsibilities; fill colour must not be used to assign different category colours under this default style. Do not change relationship-line or label colours merely while standardizing class-box fills.

After editing, enumerate every `UMLClassView` and confirm its `FillColor` is `$00B9FFFF` before delivery.

## Sequence-Diagram Actors

For every human or external-system participant in a generated WhiteStarUML sequence diagram, use a real `UMLActor` classifier. Do not create an unclassified `UMLObject` merely because it can display a name: it renders as an ordinary object instead of the standard stick-person actor.

- Reuse a matching actor classifier when it already exists in the use-case model; otherwise create a new `UMLActor` in that model.
- The participating `UMLObject` must contain `REF Classifier` pointing to the actor classifier, rather than an independent `Name` attribute.
- Add the participant instance to the actor's `Instances[...]` references and update `#Instances`.
- Keep the object's `CollaborationInstanceSet` reference, view/lifeline references, and message sender/receiver references intact.
- Verify after writing that every intended actor participant has a valid `Classifier` reference to an element of type `UMLActor`, and that the actor's reverse `Instances[...]` reference points back to that participant.

## Class and Member Structure

For each class, keep its model node and class-view node connected through their model/view references. Attributes, operations, and parameters must be owned by the appropriate classifier or operation and must also have matching compartment/view structures where the format expects them.

Important collections and counts commonly include:

- owned elements and owned views;
- attributes and operations;
- operation parameters;
- diagram views and connections;
- association, dependency, and generalization references.

The declared `#...` count must equal the number of direct child or reference entries in that collection. Do not estimate counts.

## Independent Per-Use-Case Class Models

WhiteStarUML class views render the members of the classifier model they reference. Therefore, adding a member to a shared classifier can make that member appear in every existing diagram using the classifier.

When a new per-use-case class diagram must show a different subset of members while preserving existing diagrams, create an independent model/package with its own class classifiers and class diagram. Retain the team's canonical names, types, and included members, but do not mutate the old classifier merely to build the new diagram. Treat a global update to shared classifiers as a separate, explicit user decision.

## Association

A complete association normally requires:

- one association model element;
- two association-end model elements;
- references from both endpoint classifiers to the association ends;
- role names, navigability, aggregation kind, and multiplicities as required;
- one association view owned by the diagram;
- the view's name, stereotype, property, endpoint-role, multiplicity, property, and qualifier subviews expected by the surrounding file version;
- valid model and endpoint references on the view;
- a diagram connection reference.

Do not create a shortened association view merely because it produces valid XML. WhiteStarUML may dereference missing subviews and crash.

## Preventing WhiteStarUML Access-Violation Crashes

WhiteStarUML can crash with an “Access violation … Read of address …” error even when the XML parses and all GUIDs resolve. A common cause is an incomplete **relationship presentation structure**: the semantic relationship exists, but its diagram view is missing child labels, endpoint views, or the `#Views` references that WhiteStarUML assumes are present.

### Generate relationship views from the contract

Do not create a shortened relationship view. Build the full model/view structure specified below, using unique GUIDs, the intended endpoints, text, geometry, and style.

- A `UMLDependency` or `UMLRealization` normally has four model `#Views`: the edge view plus `NameLabel`, `StereotypeLabel`, and `PropertyLabel`.
- A `UMLAssociation` also needs those four model views. In addition, **each association end** normally has four views: its role-name label, multiplicity label, property label, and qualifier compartment.
- A `UMLAssociationView` must include the complete child-view set: `NameLabel`, `StereotypeLabel`, `PropertyLabel`, `HeadRoleNameLabel`, `TailRoleNameLabel`, `HeadMultiplicityLabel`, `TailMultiplicityLabel`, `HeadPropertyLabel`, `TailPropertyLabel`, `HeadQualifierCompartment`, and `TailQualifierCompartment`.
- Every `#Views` count must exactly match its direct references. Each reference must point to the corresponding edge, label, or endpoint child view in the same diagram.

For every relationship view, preserve the working template's required fields such as `Model`, `Head`, `Tail`, `Points`, `LineColor`, `FillColor`, and the label `Visible`, `Alpha`, `Distance`, and `Model` fields. Do not assume an omitted field is optional just because XML parsing succeeds.

### Exact XPD relationship structure contract

Use the following as the required structure for this WhiteStarUML XPD family. Values in angle brackets are placeholders and must be replaced by unique, resolved GUIDs; the order of `Views[n]` references must be retained.

```text
Dependency or Realization semantic model
  #Views = 4
  Views[0] -> <edge-view-guid>
  Views[1] -> <name-label-guid>
  Views[2] -> <stereotype-label-guid>
  Views[3] -> <property-label-guid>

DependencyView or RealizationView
  required attributes: LineColor, FillColor, Points
  required references: Model -> <relationship-model>, Head -> <head-class-view>, Tail -> <tail-class-view>
  required child views, in this order:
    NameLabel       : EdgeLabelView
    StereotypeLabel : EdgeLabelView
    PropertyLabel   : EdgeLabelView
  each child label requires: Visible, Alpha, Distance, Model -> <relationship-model>

Generalization semantic model
  #Views = 4
  required references: Namespace, Child -> <subclass>, Parent -> <superclass>
  Views[0] -> <edge-view-guid>
  Views[1] -> <name-label-guid>
  Views[2] -> <stereotype-label-guid>
  Views[3] -> <property-label-guid>
  reverse references: Child.Generalizations[n] -> <generalization>; Parent.Specializations[n] -> <generalization>

GeneralizationView
  required attributes: LineColor, FillColor, Points
  required references: Model -> <generalization-model>, Head -> <parent-class-view>, Tail -> <child-class-view>
  required child views, in this order: NameLabel, StereotypeLabel, PropertyLabel
  each child label requires: Visible, Alpha, Distance, Model -> <generalization-model>
```

```text
Association semantic model
  #Views = 4
  #Connections = 2
  required reference: Namespace -> <owning-package>
  Views[0] -> <association-edge-view-guid>
  Views[1] -> <name-label-guid>
  Views[2] -> <stereotype-label-guid>
  Views[3] -> <property-label-guid>

Each UMLAssociationEnd (head and tail; repeat independently)
  #Views = 4
  Views[0] -> <role-name-label-guid>
  Views[1] -> <multiplicity-label-guid>
  Views[2] -> <property-label-guid>
  Views[3] -> <qualifier-compartment-guid>

UMLAssociationView
  required attributes: LineColor, FillColor, Points
  required references: Model -> <association-model>, Head -> <head-class-view>, Tail -> <tail-class-view>
  required direct child views, in this order:
    NameLabel, StereotypeLabel, PropertyLabel,
    HeadRoleNameLabel, TailRoleNameLabel,
    HeadMultiplicityLabel, TailMultiplicityLabel,
    HeadPropertyLabel, TailPropertyLabel,
    HeadQualifierCompartment, TailQualifierCompartment
  NameLabel/StereotypeLabel/PropertyLabel: EdgeLabelView, Model -> <association-model>
  Head/Tail role, multiplicity, and property labels: EdgeLabelView, Model -> their corresponding UMLAssociationEnd
  Head/Tail qualifier compartments: UMLQualifierCompartmentView, Model -> their corresponding UMLAssociationEnd
```

For every relationship, add the model's reverse references too: client/supplier dependency collections for a dependency or realization; both endpoint classifiers' association-end collections for an association; and the owning diagram's connection reference. Do not deliver a relationship until every listed model, view, child-view, and reverse reference is present exactly once.

### Self-contained relationship validation (no reference UML required)

Validate the generated file against this checklist, not by comparing it with another `.uml` file:

1. Parse XML; build one GUID registry; reject duplicate GUIDs and every unresolved `REF`.
2. For every `UMLDependency`, `UMLRealization`, and `UMLGeneralization`: require `#Views = 4`, exactly `Views[0]` through `Views[3]`, one edge view, and exactly three direct edge-label children in the specified order.
3. For every `UMLAssociation`: require `#Views = 4`, `#Connections = 2`, exactly two direct `UMLAssociationEnd` children, and two endpoint class references.
4. For each association end: require `Association`, `Participant`, `Multiplicity`, `#Views = 4`, and exactly `Views[0]` through `Views[3]` pointing respectively to its role, multiplicity, property, and qualifier views.
5. For every association edge: require exactly the eleven direct child views listed above, with each base label bound to the association model and each head/tail label or qualifier bound to the correct association end.
6. Verify reverse references: dependency client/supplier collections; generalization child/parent collections; association-end entries on both participant classifiers; and the relationship view in the owning diagram's connection collection.
7. Verify every declared `#Views`, `#Connections`, `#OwnedViews`, and other changed collection count equals its direct members/references exactly once.

This structural check is the required pre-delivery validation. It requires only the generated file itself; another existing UML file or desktop-app control is not required.

### Safe generation and repair flow

1. Start from the target file's root namespace/profile and generate the complete relationship structures defined in this reference.
2. Add classes and diagrams first; then add one complete relationship type at a time.
3. Run structural validation: XML parse, duplicate GUIDs, unresolved GUID references, all declared collection counts, relation `#Views`, association-end `#Views`, and expected child-label names.
4. Record the completed XML and self-contained structural checks with the output.
5. If a user reports a crash, keep a copy of the failing file and run the self-contained relationship validation above. Report the failed invariant: child name, `#Views`/`#Connections` count, unresolved GUID, endpoint, or reverse reference.
6. Make repairs idempotent, or explicitly remove duplicate generated labels and references before retesting. Re-running a non-idempotent repair can create duplicate labels while leaving a superficially valid XML file.

## Generalization

A complete generalization normally requires:

- one generalization model element with child and parent references;
- a generalization reference in the child's generalizations collection;
- a specialization reference in the parent's specializations collection;
- one generalization view with its expected name, stereotype, and property subviews;
- valid head/tail or endpoint references consistent with nearby working examples;
- a diagram connection reference.

The hollow triangle must point to the superclass.

## Dependency

A complete dependency normally requires:

- one dependency model element with client and supplier references;
- an entry in the client's client-dependencies collection;
- an entry in the supplier's supplier-dependencies collection;
- one dependency view with its expected name, stereotype, and property subviews;
- valid endpoint references;
- a diagram connection reference.

The dashed arrow points from the client to the supplier.

## Primary and Composite Keys

WhiteStarUML class diagrams may not provide database-key semantics directly. By default, leave `{PK}`, `{FK}`, and `{PK, FK}` out of attribute text, and report keys in the user-facing explanation after generation. Add visual key markers only when the user or the project's established convention explicitly requires them. When marking a composite key, mark each participating attribute, and mark foreign-key participation separately when required.

Do not substitute ERD-only objects inside a class diagram unless the user explicitly asks for an ERD.

## Abstract Formatting

The tool may italicize a classifier or operation when its `isAbstract` property is enabled. Enable it only when the model element is semantically abstract and the chosen notation permits italics. If the team wants plain text, preserve a non-abstract presentation and use the agreed textual constraint instead.

## Integrity Checks

Run all applicable checks before delivery:

- XML parses without errors;
- no duplicate GUID exists;
- every reference resolves to an existing GUID;
- every declared collection count matches its contents;
- each new class/member/relationship belongs to the intended package and diagram;
- every relationship has both model and view structures;
- all endpoints reference the intended classifiers/views;
- no unrelated existing element was renamed, deleted, or reparented;
- a backup or separate output remains recoverable.

Passing XML validation alone is insufficient; report both XML parsing and the full self-contained XPD structural validation.
