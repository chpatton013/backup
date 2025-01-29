## Goals

Backup and restore files and directories with the following features:
* data deduplication
* revision history
* small file grouping
* large file chunking
* data compression
* data and metadata encryption

## Architecture

Backup targets will be logically described in groups called "vaults", which represent a filesystem directory and associated configuration/metadata.
Backup targets will be discovered by scanning top-level vault directories, and applying a sequence of transformations defined by the vault's configuration.
Transformations can be applied to the target discovery process, changing which targets are processed by the backup tool.
Transformations can be applied to targets after they have been discovered, changing either their data, metadata, or both.
Targets are backed up to sinks after undergoing transformation. Each applied transformations is tracked in target metadata.
When targets are restored to a source, they undergo equivalent reverse-transformations in the opposite order they were applied.

[discovery] -> [transformation] -> [transfer]

## Design

TargetSource: proposes targets (and their metadata) to the discovery plugin stack
TargetDiscoveryPolicy: informs the TargetSource how to discover targets
TargetTransformation: convert target into key/value data
StorageSink: inters key/value data
