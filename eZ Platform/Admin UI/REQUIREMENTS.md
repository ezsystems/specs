# v2 Hybrid Prototype Admin UI Requirements
## https://github.com/ezsystems/specs/issues/4

# Objective
"As an eZ Engineer, I want...

To create a functioning, experimental combination of Symfony3, eZ Platform, and
a mix of existing elements (in YUI) and rewritten elements (in a new framework),
for the purpose of research and development (prototype), so that I can get a
better picture of how such a hybrid approach may work."

# Definitions
- "Hybrid Approach": A Multi-Page App (MPA) approach, replacing current SPA
  - Uses Symfony routing
  - Allows extending Admin UI via Symfony/Twig
  - Includes JavaScript enhancements utilizing modern framework(s) & librar(y/ies)

# Firm
Prototype *must*:
- Demonstrate routing in Symfony3
- Demonstrate use of a non-YUI JavaScript enhancement within Admin UI
- Demonstrate PHP API usage in Symfony3
- Demonstrate REST API usage in JavaScript enhancement
- Demonstrate re-use of an existing YUI component within

# Preferred
Prototype _should_:
- 
-

# Bonus
Prototype may:
- Demonstrate encapsulation of existing YUI elements, as well as others, in webcomponents
-

# Exclusions
Prototype *must not*:
- Rely on YUI for Admin UI

Prototype _should not_:
- Be pretty. This is just to test, play, and learn. Nothing fancy.
