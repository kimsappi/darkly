# VirtualBox settings
1. System->Processor-> enable PAE
2. Host-only Adapter, name vboxnet0

# Notes to self
## Footer
* The copyright link points to ?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c (the albatross page), which feels odd

## Feedback
* The **name** field allows HTML injection, but not XSS. Simply adding a `<script>` block does not return a flag (the tags are filtered out).
