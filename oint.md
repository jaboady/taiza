Après nettoyage des répertoires state, work et logs, nous avons effectué deux redémarrages complets de Liferay.

Le problème reste reproductible à chaque démarrage.

La requête sur BatchEngineImportTask montre :

* task 1 — 30/09 11:07 — ObjectDefinition — COMPLETED
* task 2001 — 30/09 13:08 — FAILED
* task 4001 — 01/10 12:36 — FAILED
* task 6001 — 01/10 13:28 — FAILED

Les trois échecs présentent exactement le même message :
Cannot invoke "Object.hashCode()" because "key" is null

Le traitement échoue avant l’import des ObjectDefinitions (processedItemsCount = 0, totalItemsCount = 4).

Nous avons également vérifié que le contenu du batch ayant réussi (task 1) et celui du batch en échec (task 2001) est identique.

Après démarrage, com.liferay.batch.engine.service version 4.0.134 est ACTIVE et ConfigurationProviderImpl est bien enregistré comme service OSGi.

La stack trace passe notamment par BatchEngineImportTaskExecutorImpl._getCSVFileColumnDelimiter(), ConfigurationProviderImpl.getCompanyConfiguration(), SettingsLocatorHelperImpl.getConfigurationPidMapping() puis ServiceTrackerMapImpl, avant le NPE.