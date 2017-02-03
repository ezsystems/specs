# v2 Hybrid Prototype Admin UI Requirements
## https://github.com/ezsystems/specs/issues/4

# Objective
"As an eZ Engineer, I want...

To create a functioning, experimental combination of Symfony3, eZ Platform, and
a mix of existing elements (in YUI) and rewritten elements (in a new framework),
for the purpose of research and development (prototype), so that I can get a
better picture of how such a hybrid approach may work."

Underlying main goals are:
- by adopting Symfony 3, we will provide developers, and especially the one who are experienced with eZ Publish or Symfony, substantially faster and easier way to develop, extend and customize eZ Platform.
- by insuring an hybrid architecture that reuses existing elements relying on YUI, we will speed up availability of 2.x by not having to redevelop all features. Candidates for reuse are UDW, Sub-items view, Content view. If this is not possible, expectations and plans for 2.x will have to be be re-adjusted.

# Definitions
- "Hybrid Approach": A Multi-Page App (MPA) approach, replacing current SPA
  - Uses Symfony routing and insure we can have application URL that can use content ids, location ids, content fields (name)
  - Allows extending Admin UI via Symfony/Twig
  - Includes JavaScript enhancements utilizing modern framework(s) & librar(y/ies)

# Firm
Prototype *must*:
- Demonstrate routing in Symfony3
- Demonstrate PHP API usage in Symfony3
- Demonstrate re-use of an existing YUI component within new solution
- Demontsrate re-use of existing non-YUI component such as Content Type View (which is Symfony based)

# Preferred
Prototype _should_:
- Demonstrate use of a non-YUI JavaScript enhancement within Admin UI
- Use the Universal Discovery Widget as the test subject to Demonstrate re-use of existing YUI component within new solution
- ...

# Bonus
Prototype may:
- Demonstrate encapsulation of existing YUI elements, as well as others, in webcomponents
- ...

# Exclusions
Prototype *must not*:
- Rely on YUI for Admin UI

Prototype _should not_:
- Be pretty. This is just to test, play, and learn. Nothing fancy.
