# Savvy Widget

Savvy allows users to quickly and easily find savings on their existing insurance.

Your website or mobile app can easily embed the "Savvy Widget" so that users can find these savings without ever navigating off your property.

## Basic Usage

```html
<script src="https://cdn.savvy.insure/sdk/v1.0/savvy.js"></script>
<a href="#" id="openSavvyBtn">Check My Policy for Savings</a>
<script>
  (function () {
    var handler = Savvy.configure({
      urlTrackingParams: 'REPLACE-THIS-WITH-STRING-FROM-SAVVY',
    });
    document.getElementById('openSavvyBtn').onclick = handler.open;
  })();
</script>
```

## Advanced Usage

Savvy Widget supports a number of Javascript callbacks that you can use for analytic purposes and to personalize the user experience around the progress made in the Savvy Widget.

```html
<script src="https://cdn.savvy.insure/sdk/v1.0/savvy.js"></script>
<a href="#" id="openSavvyBtn">Check My Policy for Savings</a>
<script>
  (function () {
    var handler = Savvy.configure({
      // REQURIED: Use the tracking/attribution params provided by your contact at Savvy.
      urlTrackingParams: '?utm_source=YOUR_ID&utm_medium=incentive',

      // OPTIONAL: Your Trellis Client ID. Required if you intend to collect end-user PII.
      trellisClientId: API_CLIENT_ID,

      // OPTIONAL: onConnect(connectionId, metadata)
      // Called when the user has authenticated access to their insurance account
      // and granted permission to Savvy to access its data.
      // - connectionId - Set to null if trellisClientId not provided.
      //                  Used for accessing user PII from Trellis API.
      onConnect: handleConnect,

      // OPTIONAL: onClose(error, metadata)
      // Called when the user closes the modal dialog.
      // - metadata
      //   - metadata.accountReferenceId - Account reference ID to use for searching Savvy.
      //                                   Currently set equal to Trellis Connection ID when
      //                                   Trellis Client ID is provided.
      //                                   May not be present if our servers could not be reached.
      onClose: handleClose,

      // OPTIONAL: onEvent(eventName, metadata)
      // Called when certain events happen. Supported event names:
      // - OPEN
      //     - The user has opened the widget.
      // - SELECT_ISSUER
      //     - The user has selected their insurance company.
      // - AUTH_COMPLETE
      //     - The user has finished login/authentication for an account.
      // - PROVIDE_CONSENT
      //     - The user has consented to Savvy's searching for offers.
      // - APPLICATION_COMPLETE
      //     - Enough data has been received that Savvy can get personalized offers.
      // - LOAD_OFFERS
      //     - The user has finished waiting for offers.
      // - CLICK_OFFER
      //     - The user clicked on a specific offer.
      // - TRANSITION_VIEW
      //     - When the Widget transitions between views.
      // - CLOSE
      //     - The flow has been exited. Also calls `onClose` callback.
      // - ERROR
      //     - If an error happens during the flow.
      onEvent: handleSavvyEvent,
    });
    document.getElementById('openSavvyBtn').onclick = handler.open;
  })();
</script>
```

### destroy() function

The destroy function allows you to destroy the Savvy handler instance, properly removing any DOM artifacts that were created by it. This function will fail if called when Savvy Widget is open.

```html
<script>
  // Create the Savvy handler
  var handler = Savvy.configure({
    urlTrackingParams: 'REPLACE-THIS-WITH-STRING-FROM-SAVVY',
  });

  // Destroy handler
  handler.destroy();
</script>
```

## CHANGELOG

- 2020-04-28
  - Update "onClose" handler to pass metadata that includes accountRefId.
- 2020-02-13 – Initial draft
