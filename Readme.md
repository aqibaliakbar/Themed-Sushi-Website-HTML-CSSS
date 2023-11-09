npm create vite@latest ./
npm install
npm run dev

==========

**For Animation**

npm install aos

**What is BEM?**

BEM is a front-end naming method for organizing and naming CSS classes. The Block, Element, Modifier methodology is a popular naming convention for class names in HTML and CSS. It helps to write clean CSS by following some simple rules.

Projects of any size with CSS can benefit from a BEM framework—unless writing styles directly inside already organized JavaScript files and using Styled Components or something similar. To complement BEM, include some parts of SMACSS which act as a style guide for naming conventions. BEM is easy to read and organizes styles, even when projects are extremely large.

Class naming has never been an easy part of CSS. It’s challenging for outside developers to understand what developer-specified classes do. Naming conventions can quickly get out of control when refactoring code more than once and when working with a team.

Some of the most irritating problems related to naming in CSS when:

Overriding classes accidentally.

Hard to read both CSS and HTML has no clear scoping and structure in naming.

New team members waste time learning about or trying to find already created classes.

The code is hard to update because it’s not clear what aspects are global and what belongs to the local component.

It’s unclear how to handle nesting (i.e., what to add or keep?).

Most problems come from poor naming and lack of scope (aka the scope that closes the context). For example, when reading the code of the menu developers should feel confident that the code does not include any styles of the headings. In this article, I’ll go over what BEM is, how it works, and recommendations on how to implement BEM successfully.

How to implement BEM
There are three main parts of BEM.

Block which holds everything (elements) inside and acts as a scope.

Element which acts as a specific part of the component.

Modifier which adds additional styles to a specific element(s).

The information and examples below detail how to implement BEM successfully.

As shown in code snippet 1, a block .head holds all two elements named .head\_\_eye. The element name has block name at the beginning, followed by two underscores and then goes the element name itself. The element name is just an element name and cannot have underscores. When using multiple words include “-” hyphen-minus to separate them. The modifier name has a block name at the beginning, two underscores, an element name (just like naming the element), and then two hyphen-minuses with a modifier name.

Code snippet 1

<div class="head">
  <div class="head__eye head__eye--left">(o)</div>
  <div class="head__eye head__eye--right">(o)</div>
</div>
Two recommended approaches for modifiers: One option on a block level, another on an element level
In code snippet 2, a .head--small is used as a block-level modifier which means that developers can apply styles to the block itself and its elements based on a block modifier.

Code snippet 2

<div class="head head--small">
  <div class="head__eye">(o)</div>
  <div class="head__eye">(o)</div>
</div>
.head {
  height: 5em;

&\_\_eye {
height: 0.5em;
}

&--small {
height: 80%;

    .head__eye {
      height: 0.4em;
    }

}
}
Code snippet 3 shows an example of SCSS syntax with the usual styles for the head and styles for a small head.

Code snippet 3

<div class="head">
  <div class="head__eye head__eye--small">(o)</div>
  <div class="head__eye">(o)</div>
</div>
.head {
  height: 5em;

&\_\_eye {
height: 0.5em;

    &--small {
      height: 0.4em;
    }

}
}
If the preference is to make just one eye small, the above code would not work and would require adding an element level modifier as shown in code snippet 3.

Do not leave a modifier alone. Always keep modifiers together with the element class as noted in code snippet 3. .head**eye should always appear before the .head**eye--small and on the same HTML element.

Avoid managing the state with modifiers.
I recommend leveraging SMACCS (another CSS naming convention) with state classes .is-active, .is-open, and .is-valid for clarity. Code snippet 4 depicts a state clearly.

Code snippet 4

<div class="head">
  <div class="head__eye is-closed">(o)</div>
  <div class="head__eye">(o)</div>
</div>
.head {
  &__eye {
    height: 0.5em;

    &.is-closed {
      height: 0;
    }

}
}
Include one level for block, one level for elements.
In code snippet 5, everything looks easy when using two levels (i.e., block on a first level and elements on a second level). When more levels or more elements are needed, simply keep the original rule of naming the elements. Do not include additional underscores like .head**eye**eyball which breaks the rule. Use mindful naming and everything looks clear when reading the HTML.

Code snippet 5

<div class="head">
  <div class="head__eye">
    <div class="head__eye-eyeball">
      <div class="head__eye-iris"></div>
    </div>
  </div>
</div>
As an alternative, leave .head__iris instead of .head__eye-iris because block part in this elements’ name show that this elements’ scope is .head. Add as many levels as necessary and the CSS renders one class per one element. Another option is to start also new blocks inside elements. Most times, the block has elements for the layout and other blocks for common elements.

Use modifiers when mixing components.
When noting one div and multiple block classes to it, use modifiers. It will always be cleaner. Do not mix multiple blocks.

Code snippet 6

<div class="footer">
  <div class="message message--inside-footer">
    <p class="message__text">Message</p>
  </div>
</div>
Code snippet 6 shows a message with slightly different styles when it is in footer compared to what styles it has when it is inside content. Don’t use a .footer__message and .message classes on one div (by adding an element and a new block on the same div). Instead, use a modifier that is much cleaner for reading the code and styling.

Handle multiple modifiers for the same element
Having a hard time figuring out what to do before writing eight modifiers for the same element? When the CSS code appears hard to read and is a long file, it may indicate the need to inspect the block and consider creating a new component. However, it can also mean that there are too many blocks written in one file and the code needs some refactoring.

What could go wrong? Too many modifiers are hard to manage, especially, when one element includes too many modifiers. The code becomes difficult to read with some modifiers becoming blocks. The easiest way to keep things clean is to start with modifiers and split the block into more blocks.

Build a component inside a component
Sometimes the parent component dictates style variations for some of the child components. As shown in image 9, use a global component for displaying a box type of content with the title, description, and parent. Include a modifier for news**item. Then, when writing we the style for this structure, also write a selector .news**item--alternative .box**item-title. The code looks like a hack for selecting a component thought a parent components modifier. However, the easy way to resolve the issue is to move the modifier to a box component from news**item--alternative to box\_\_item-title--alternative.

Code snippet 7

<div class="news">
  <div class="grid">
    <div class="grid__item">
      <div class="news__item news__item--alternative">
        <div class="box">
          <div class="box__item-title">Title</div>
          <div class="box__item-description">...</div>
        </div>
      </div>
    </div>
    <div class="grid__item">
      ...
    </div>
  </div>
</div>
Sometimes, this isn’t realistic because of existing limitations (e.g., making the app too complicated by passing additional arguments just for styles). Keep in mind that BEM is a methodology, not a strict rule. Try all the documented options noted first to save time and energy in the future. Sometimes it’s easier to break down the components into smaller pieces or introduce new components. If there is no other option, use selectors that don’t look nice as a last resort.

It’s okay not to use BEM
A global helper class (e.g., .hidden) is acceptable to use because it does what it says it will do. The text hides elements. As an option, create a pattern for the global classes and use them like .g-hiden with .g representing “global”. Or if the modifier needs to perform the same action on multiple elements, create a new pattern for these cases, like .head-alternative-coloring.

It’s hard to think of a scenario when clean BEM poses problems. Even small projects are easier to read when using BEM. However, when using styled-components, CSS modules or something similar, BEM isn’t needed. In these instances, scoping every component occurs already. Mindful naming should be enough in most cases.

In conclusion
There are many options to choose from when writing CSS. Some prefer to write clean styles in pure CSS. Others prefer to use methodology like BEM. No matter the choice, styles need to be readable and maintainable. Consider using some methodology like BEM for an easy way to keep the code clean and clear.
