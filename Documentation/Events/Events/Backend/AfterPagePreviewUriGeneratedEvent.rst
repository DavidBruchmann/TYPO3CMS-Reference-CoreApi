.. include:: /Includes.rst.txt
.. index:: Events; AfterPagePreviewUriGeneratedEvent
.. _AfterPagePreviewUriGeneratedEvent:


=================================
AfterPagePreviewUriGeneratedEvent
=================================

.. versionadded:: 12.0
   This PSR-14 event replaces the
   :php:`$GLOBALS['TYPO3_CONF_VARS']['SC_OPTIONS']['t3lib/class.t3lib_befunc.php']['viewOnClickClass']`
   postProcess hook.

The :php:`\TYPO3\CMS\Backend\Routing\Event\AfterPagePreviewUriGeneratedEvent`
is executed in :php:`\TYPO3\CMS\Backend\Routing->buildUri()`, after the preview
URI has been built - or set by an event listener to
:ref:`BeforePagePreviewUriGeneratedEvent`. It allows to overwrite the built
preview URI. This event however does not feature the possibility to modify the
parameters, since this won't have any effect as the preview URI is directly
returned after event dispatching and no further action is done by the
:php:`PreviewUriBuilder`.

API
===

.. include:: /CodeSnippets/Events/Backend/AfterPagePreviewUriGeneratedEvent.rst.txt

Example
=======

Registration of the Event in your extensions' :file:`Services.yaml`:

.. code-block:: yaml
   :caption: EXT:my_extension/Configuration/Services.yaml

   MyVendor\MyExtension\Backend\MyEventListener:
     tags:
       - name: event.listener
         identifier: 'my-extension/backend/modify-preview-uri'
         method: 'modifyPreviewUri'

The corresponding event listener class:

.. code-block:: php
   :caption: EXT:my_extension/Classes/Backend/MyEventListener.php

   use TYPO3\CMS\Backend\Routing\Event\AfterPagePreviewUriGeneratedEvent;

   final class MyEventListener
   {
       public function modifyPreviewUri(AfterPagePreviewUriGeneratedEvent $event): void
       {
           // Add custom fragment to built preview URI
           $uri = $event->getPreviewUri();
           $uri = $uri->withFragment('#customFragment');
           $event->setPreviewUri($uri);
       }
   }
