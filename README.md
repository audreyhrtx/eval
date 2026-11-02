# Evaluation - Ordonnancement d'un pipeline

La solution s'appelle `Pipeline` et contient :

- `Pipeline/Pipeline.csproj` : bibliothèque de classes ;
- `Pipeline.Worker/Pipeline.Worker.csproj` : Worker Service ;
- `Pipeline.Tests/Pipeline.Tests.csproj` : tests unitaires xUnit.

## Lancer les tests

```powershell
dotnet test .\Pipeline.sln
```

## Utiliser le Worker

Par défaut, le Worker utilise le fichier local
`pipeline.example.json`, ce qui permet de le lancer immédiatement :

```powershell
dotnet run --project .\Pipeline.Worker\Pipeline.Worker.csproj
```

Le Worker télécharge un fichier JSON depuis l'URL configurée dans
`Pipeline.Worker/appsettings.json` (`PipelineUrl`) ou dans la variable
d'environnement `PIPELINE_URL`.

```powershell
$env:PIPELINE_URL = "https://exemple.fr/pipeline.json"
dotnet run --project .\Pipeline.Worker\Pipeline.Worker.csproj
```

Le planning est calculé par groupes : les tâches d'un même groupe peuvent
être exécutées en parallèle, tandis que chaque groupe attend la fin du
précédent. Les dépendances circulaires déclenchent une
`InvalidOperationException` et un pipeline vide déclenche une
`ArgumentException`.
