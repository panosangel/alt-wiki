# Shadow DOM Notes

- Styles from outside don't leak to children of the shadow DOM root.
- Slot content is styles from outside the shadow DOM.
- Slot content can be styled inside the shadow DOM with `::slotted(*)` but only the top most element.
- Slot content outside style will overwrite the inner one. Light DOM wins.
- Web component can be styled by tag name in the light DOM.
- Web component can be styled from inside with `:host`. Light DOM wins.
- Use CSS variables set `:root` or `html`.
