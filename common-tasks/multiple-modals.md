# Multiple modals handling

**First try**

I created a re-useable modal wrapper component that contains a trigger and the modal. Modal component is built using Radix-ui

```jsx
type Props = {
  triggerElem: ReactNode;
};

const ModalWrapper = ({ triggerElem, children }: PropsWithChildren<Props>) => {
  // Using open to lazy load the modal
  const [open, setOpen] = useState(false);
  return (
    <div>
      <CustomDialog open={open} onOpenChange={setOpen}>
        <CustomDialogTrigger className="text-left w-full">
          {triggerElem}
        </CustomDialogTrigger>
        {open && <Suspense>{children}</Suspense>}
      </CustomDialog>
    </div>
  );
};
```

And you can use this component anywhere you want to place a modal.

## The problem

Assume you have two modals: A and B. Modal A has a button that, when clicked, activates Modal B. As a result, modals A and B appear concurrently.

This is not what we want. We need A to disappear when modal B is active

So, we still need a store or context to handle the appearance state of the modals. The disadvantage of this strategy is that we must control the number of modals manually.