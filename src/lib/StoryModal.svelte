<script>
    const {
      // provided by <Modals />
      isOpen,
      close,
  
      // your props
      title,
      message
    } = $props()
  
  </script>
  
  {#if isOpen}
    <div role="dialog" class="modal">
      <div class="contents max-w-md">
        <h4 class="h4">{title}</h4>
        <p>{message}</p>
        <div class="actions">
          <button class="btn preset-filled-primary-500" onclick={() => close()}>Got it</button>
        </div>
      </div>
    </div>
  {/if}
  
  <style>
    .modal {
      position: fixed;
      top: 0;
      bottom: 0;
      right: 0;
      left: 0;
      display: flex;
      justify-content: center;
      align-items: center;
  
      /* allow click-through to backdrop */
      pointer-events: none;
    }
  
    .contents {
      min-width: 240px;
      padding: 16px;
      background: white;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      pointer-events: auto;
    }
  
    p {
      text-align: center;
      margin-top: 16px;
      border-radius: 6px;
    }
  
    .actions {
      margin-top: 32px;
      display: flex;
      justify-content: flex-end;
    }
  
  </style>