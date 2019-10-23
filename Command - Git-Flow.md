# Command - Git-Flow

## Installation
- Debian/Ubuntu:
    ```bash
    apt-get install git-flow
    ```
- macOS:
    ```bash
    brew install git-flow-avh
    ```

## Initialize
```bash
git flow init 
```

## Start a new feature
This action creates a new feature branch based on 'develop' and switches to it.
```bash
git flow feature start <feature_name>
```

## Publish a feature
Are you developing a feature in collaboration?  
Publish a feature to the remote server so it can be used by other users.
```bash
git flow feature publish <feature_name>
```

## Getting a published feature
 Get a feature published by another user.
```bash
git flow feature pull origin <feature_name>
```
You can track a feature on origin by using
```bash
git flow feature track <feature_name>
```

## Rename a feature
```bash
git flow feature rename <feature_name>
```

## Delete a feature
```bash
git flow feature delete <feature_name>
```

## Finish a feature
```bash
git flow feature finish <feature_name>
```

## Appendix A - Sources:
- [git-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)
