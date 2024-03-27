:og:image: https://docs.getodk.org/_static/img/tutorial-community-reporting.png

Entities tutorial: Community reporting
========================================

De nombreuses organisations utilisent des formulaires pour recueillir divers signalements de la part de leurs communautés :

* Les hôpitaux demandent aux patients et aux soignants de signaler les problèmes d'insalubrité ou de fournitures manquantes.
* les réprésentants des étudiants demandent aux autres étudiants de faire des suggestions d'améliorations de leur école
* Les munbicipalités demandent à leurs habitants de signaler les lampadaires cassés, les arbres tombés, etc.

Dans ce tutoriel, vous utiliserez les entités pour mettre en place un flux de travail dans lequel les habitants d'une ville signalent des problèmes, que les employés de la ville peuvent ainsi résoudre.

.. seealso::
   `Vidéo des coulisses <https://youtu.be/919SIU41UQA>`_ du développement de ce tutoriel.

Objectifs
-----

* Créer des entités à partir de soumissions de formulaires
* Accéder aux entités dans formulaire de suivi
* Mettre à jour des entités à partir de soumissions de formulaires

.. _tutorial-entities-capture-problem:

Saisir les signalements de problèmes à l'aide d'un simple formulaire
-------------------------------------------

.. seealso::
    Si vous n'avez pas encore l'habitude de construire des formulaires XLSForms, commencez par :doc:`xlsform-first-form`.

Un formulaire de signalement peut être très simple. Pour que les employés municipaux puissent trouver et résoudre les problèmes signalés, il suffit de saisir un titre, une description du problème et un lieu. Pour cela, vous pouvez créer votre propre formulaire ou utiliser notre exemple `use our example <https://docs.google.com/spreadsheets/d/1zhnRnjD3ZH_OwARAE1hY4__8nFta1LauCPaZbWyI2ag/edit#gid=1068911091>`_.

.. image:: /img/tutorial-community-reporting/problem-report-simple.*
    :alt: Un formulaire simple de signalement de problème. Il enregistre le titre du problème, sa desciption et son emplacement.

Vous pouvez vous arréter ici et construire une chaine de traitements autour de ce simple formulaire. Par exemple, vous pourriez publier le formulaire avec un lien d'accès public :ref:`Lien d'Accès Public <central-submissions-public-link>`, créer un QR code contenant ce lien (en utilisant un service comme `Celui d'Adobe' <https://new.express.adobe.com/tools/generate-qr-code>`_), et l'afficher dans l'espace public afin que les habitants s'en servent. Alors les employés municipaux pourront :ref:`afficher les soumissions du formulaire dans Excel <central-submissions-odata>` et ajouter une colonne dans une feuille de calcul partagée pour gérer la résolution des nouveaux problèmes.

Cette approche nécessite des interventions manuelles et de la coordination avec une feuille de calcul, les deux aspects pouvant être source d'erreur et chronophages. Utilisons plutôt les Entités d'ODK pour aider la gestion des problèmes en temps réel.

Créer des entités pour chaque signalement
-------------------------------------------

Nous aimerions que les signalements soit disponibles dans un autre formlulaire afin que les employs puissent les prendre en charge et résoudre les problèmes. Pour cela dans ODK, nous pouvons utiliser les entités. Une entité représente un objet unique qui peut être partagé entre différents formulaitres (voir :doc:`central-entities`).

Démarrons en prenant notre :ref:`Report a problem <tutorial-entities-capture-problem>` formulaire existant et faisons lui créer des Entités qui représenteront les problèmes signalés.

#. Ouvrez ou créez la feulle ``entities`` dans le formualire ``Report a problem``.
#. Dans la colonne ``list_name``, entrez le nom de la Liste d'Entités dans laquelle vous souhaitez créer des Entités : ``problems``. Ce nom sera généralement un nompluriel représentant une collection des objets que vous souhaitez paratger entre vos formulaires. 
#. Dans la colonne ``label``, entrez une expression qui définira l'étiquette de chaque signalement : ``${problem_title}``. Cette étiquerre sera utilisée dans Cnetral pour identifier chaque entité ainsi que dans les selections définies dans les formulaires de suivi.

Ces ajouts entraineront, à chaque soumission de formulaire, la création d'entités ``problems`` avec une étiquette définie par l'utilisateur et un identifiant unique généré automatiquement. Dans ce cas, vous voulez aussi rendre disponible les détails et la localisation du problème dans les formualires de suivi..

#. Ouvrez la feuille ``survey`` du formualire ``Report a problem``.
#. Trouvez ou ajoutez la colonne ``save_to`` (Elle n'est pas présente par défaut dans le :doc:`XLSForm template <xlsform>`).
#. Dans la colonne ``save_to`` du champ de formulaire qui capture la description du signalement, entrez le nom de la propriété de l'Entité où stocker cette valeur : ``details``
#. Dans la colonne ``save_to`` du champ de formulaire qui capture la localisation du signalement, entrez le nom de la propriété de l'Entité où stocker cette valeur : ``geometry``. Utiliser le nom particulier ``geometry`` pour cette propriété vous pemrettra d'afficher les ``problems`` sur une carte dans le formualire de suivi (voir :ref:`select one from map <select-from-map>`).

.. image:: /img/tutorial-community-reporting/problem-report-entities.*
    :alt: Un formulaire simple de signalement de problèmes. Il collecte le titre du problème, sa descrption, sa localisation et créée les Entités correspondantes.

Voir le formulaire fonctionnel `Signaler un problème <https://docs.google.com/spreadsheets/d/10sVEXd3apzePPDY_SQGaEU3z3gj6H5W3RSHFWCm0HIU>`_ .

Vérifiez que la création d'Entité fonctionne
--------------------------------------------

Actuellement les entités ne peuvent être créées en mode "Ebauche de formulaire", vous devez donc publier votre formulaire pour le tester.

#. Rendez-vous dans un projet dédié aux tests de formulaires et aux tutoriels, créez en un si vous n'en avez pas (voir :ref:`the guide on testing forms <guide-testing-project>`).
   
   .. warning::
       Vous pouvez créer un projet existant contenant de vrais formulaires mais notez que les listes d'entités ne peuvent pas être supprimées pour le moement, et donc que les signalements créés pendant vos tests existeront jusqu'à ce que Central permette leur suppression.

#. Cliquez sur le bouton :guilabel:`New` et chargez votre nouveau formulaire. Selon comment vous aurez créé votre formulaire, vous devrez peut-être d'abord le télécharger puis l'exporter en XLSX.

#. Corrigez tous les problèmes identifiés lors de la conversion puis publiez le formulaire.

#. Cliquez sur l'onglet :guilabel:`Submissions` puis sur le bouton :guilabel:`New` afin d'utilise rle fomulaire web pour créer une ou plusieurs soumissions.

#. Rafraichissez la table des soumissions pour voir les nouvelles puis cliquez sur le bouton :guilabel:`More` de l'une d'entre elles pour en afficher les détails. Vous devriez voir que cette soumission a créé une Entité dans la liste ``problems`` :

   .. image:: /img/tutorial-community-reporting/problem-report-submission.*
     :alt: Détail d'une soumission du formulaire ``Report a problem`` qui a créé une Entité.

Afficher les problèmes signalés sur une carte
---------------------------------------------

Créons maintenant un second formulaire qui sera utilisé par les employés municipaux pour voir les signalements sur une carte.

#. Créez un nouveau formualire à partir du :doc:`modèle XLSForm <xlsform>`. Nommez le fichier ``Address a problem``.
#. Allez dans la feuille ``settings``.
#. Dans la colonne ``form_title``, renseignez un titre qui sera lu par les utilisateurs du formulaire : ``Address a problem``
#. In the ``form_id`` column, put an ID that uniquely identifies this form: ``address_problem``
#. Add a field list group to show multiple questions on a single screen:

   #. Allez à la feuille ``survey``.
   #. Dans la colonne ``type``, entrez ``begin_group``
   #. Dans la colonne ``name``, entrez ``entity``
   #. Dans la colonne ``appearance``, entrez ``field-list``
#. Add a select question that shows all reported problems on a map:

   #. Dans la colonne ``type``, entrez ``select_one_from_file problems.csv`` which will automatically link to the ``problems`` Entity List because of the ``problems.csv`` filename.
      
      .. warning::
         The filename specified is case-sensitive and it must exactly match the Entity List name specified in the problem reporting form or the two forms will not share Entities.

   #. Dans la colonne ``name``, entrez ``problem``
   #. Dans la colonne ``appearance``, entrez ``map``
#. Add a note question to show the selected problem's details:
  
   #. Dans la colonne ``type``, entrez ``note``
   #. Dans la colonne ``name``, entrez ``problem_details``
   #. Dans la colonne ``label``, entrez ``Details: instance('problems')/root/item[name=${problem}]/details``

      .. note::
         Don't worry if this part looks unfamiliar. Copy it as is, and after you finish the tutorial, you can learn more in the ``List lookups`` sheet in the :doc:`XLSForm template <xlsform>`.

#. Close the field list group:

   #. Dans la colonne ``type``, entrez ``end_group``
#. Upload the form to Central in the same project as the ``Report a problem`` form and try its draft. If you use the draft QR code in the Collect mobile app, you will see a map of all reported problems. If you use a web form for testing, you will see a list of reported problems by label (because select from map is not yet implemented).

You can now view reported problems! When a new problem is reported, it will appear in the follow-up form the next time the form is updated. If you're online, updates typically happen automatically every 15 minutes.

Capture information about the action taken
-------------------------------------------

You can now add to the ``Address problem`` form to capture information about any actions that the town employee takes.

#. Add a field list group to show multiple questions on a single screen:

   #. Allez à la feuille ``survey``.
   #. Dans la colonne ``type``, entrez ``begin_group``
   #. Dans la colonne ``name``, entrez ``action``
   #. Dans la colonne ``appearance``, entrez ``field-list``
#. Add a text question to get a description of the action taken:

   #. Dans la colonne ``type``, entrez ``text``
   #. Dans la colonne ``name``, entrez ``action_taken``
   #. Dans la colonne ``label``, entrez ``Describe the action you have taken``
#. Add a select question to get the new status of the problem:

   #. Dans la colonne ``type``, entrez ``select_one statuses``
   #. Dans la colonne ``name``, entrez ``status``
   #. Dans la colonne ``label``, entrez ``What is the problem status after your action?``
#. Go to the ``choices`` sheet.
#. Add a choice for actions that address the problem:

   #. Dans la colonne ``list_name``, entrez ``statuses``
   #. Dans la colonne ``name``, entrez ``addressed``
   #. Dans la colonne ``label``, entrez ``Addressed``
#. Add a choice for actions that don't fully address the problem:

   #. Dans la colonne ``list_name``, entrez ``statuses``
   #. Dans la colonne ``name``, entrez ``needs_followup``
   #. Dans la colonne ``label``, entrez ``Needs follow-up``
#. Update your draft in Central and try it out to make sure it works as intended.

Update a problem's status
--------------------------

You can now capture information about problems that are addressed or that still need action. However, this is not that useful because problems that have been addressed are still visible from the ``Address problem`` form which could lead to confusion and duplication of effort.

We need to filter out addressed problems from the select in ``Address problem`` so that it only shows problems that require action. 

Let's update a problem Entity's status when the ``Address problem`` form is filled out about it. We'll then be able to filter out ``problems`` with a ``status`` of ``addressed``.

#. Declare that this form's submissions should update Entities in the ``problems`` Entity List:

   #. Go to the ``entities`` sheet of the ``Address problem`` form.
   #. Dans la colonne ``list_name``, entrez ``problems``
   #. Delete the ``label`` column if it exists because this form does not need to update the label of ``problem`` Entities.
   #. In the ``entity_id`` column (you may need to add it), put ``${problem}`` to indicate that the value of the ``problem`` form field represents the unique identifier of the ``problem`` Entity to update.

#. Update the value of the ``status`` Entity property:

   #. Allez à la feuille ``survey``.
   #. In the ``save_to`` column (you may need to add it) for the ``status`` field, put ``status``

#. Filter out problems with a status of ``addressed``

   #. In the ``choice_filter`` column for the row of the question named ``problem``, put ``status != 'addressed'`` to indicate that only problems with a status other than ``'addressed'`` should be included.

   .. note::
     Using a filter like this means it will not be possible to edit submissions on the server because the selected Entity that was addressed by the submission will be filtered out on edit. In many Entity-based workflows, submission edits are unnecessary and can be avoided. In this workflow you can allow them by changing the choice filter to `status != 'addressed' or name = current()`.

#. Fix any form conversion errors and then publish the form. Entity updates currently only work with a published form, just like Entity creation.

.. image:: /img/tutorial-community-reporting/address-problem.*
    :alt: A form for addressing problems.

See the working `Address a problem <https://docs.google.com/spreadsheets/d/1C_WrfD4_9QuycO_pgzE8duw9kaOxAB3CfPOb0HNOQfU>`_ form.

Try out the full workflow
--------------------------

Let's report a few problems using the web form.

#. In Central, go to your project or the server landing page and then click on the ``*`` icon to the right of the ``Report a problem`` form. That icon and the number next to it represent the total number of current submissions. Clicking it will jump directly to the :guilabel:`Submissions` tab.

   .. image:: /img/tutorial-community-reporting/problem-report-project.*
    :alt: The list of forms in the project with the cursor hovering over the total submission count.

#. Click the :guilabel:`New` button to initiate a new submission.
#. Report a few problems in different locations.

You could also address problems using the web form but to get the map view, let's use the Collect mobile app.

#. Go to your project page in Central.
#. Click on the :guilabel:`App Users` tab.
#. Create a new App User with name ``Employee1``.
#. Scan the App User QR code from Collect.
#. Click on the :guilabel:`Form Access` tab.
#. Give ``Employee1`` access to the ``Address a problem`` form. You can optionally also given it access to ``Report a problem``.
#. Open the ``Address a problem`` form and address some problems! Make sure to tap the refresh button in :guilabel:`Start new form` before each problem resolution (⟳) to get the latest status updates.

You now have two forms that work together to support a problem reporting and resolution workflow that can be applied to many different environments.

.. note::
    Addressed problems are filtered out of the ``Address a problem`` select but they are still sent to all devices. This will become impractical after tens of thousands of problems. In a future ODK version, it will be possible to archive Entities that are no longer needed.

Your turn
----------

#. Can you set a ``marker-color`` Entity property to ``#FFFF00`` (yellow) if the status is set to ``needs_followup``? (hint: use a ``calculation`` with ``if``)
#. Can you set a ``marker-symbol`` Entity property to ``❗️`` if the status is set to ``needs_followup``?
#. Can you show addressed problems on the map with a ✅ symbol instead of filtering them out?
#. Can you specify a constraint to show an error when an addressed problem is selected? (note: this is incompatible with server-based submission edits, just like the original choice filter)