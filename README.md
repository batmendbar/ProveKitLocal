# ProveKit

Zero-knowledge proof toolkit targeting mobile devices.

## Demo instructions

First make sure you have the exact correct version of Noir installed [so the artifacts can be read](./Cargo.toml#L58):

```sh
noirup -C 2a79d90
```

Compile the Noir circuit:

```sh
cd noir-examples/poseidon-rounds
nargo compile
```

Generate the Noir Proof Scheme:

```sh
cargo run --release --bin noir-r1cs prepare ./target/basic.json -o ./noir-proof-scheme.nps
```

Generate the Noir Proof using the input Toml:

```sh
cargo run --release --bin noir-r1cs prove ./noir-proof-scheme.nps ./Prover.toml -o ./noir-proof.np
```

Verify the Noir Proof:

```sh
cargo run --release --bin noir-r1cs verify ./noir-proof-scheme.nps ./noir-proof.np
```

Generate inputs for Gnark circuit:

```sh
cargo run --release --bin noir-r1cs generate-gnark-inputs ./noir-proof-scheme.nps ./noir-proof.np
```

Recursively verify in a Gnark proof (reads the proof from `../ProveKit/prover/proof`):

```sh
cd ../../gnark-whir
go run .
```

Benchmark against Barretenberg:

```sh
cd noir-examples/poseidon-rounds
cargo run --release --bin noir-r1cs prepare ./target/basic.json -o ./scheme.nps
hyperfine 'nargo execute && bb prove -b ./target/basic.json -w ./target/basic.gz -o ./target' '../../target/release/noir-r1cs prove ./scheme.nps ./Prover.toml'
```

Profile

```sh
samply record -r 10000 -- ./target/release/noir-r1cs prove ./noir-proof-scheme.nps ./noir-examples/poseidon-rounds/Prover.toml -o ./noir-proof.np
```

## Components

## Dependencies

This project depends on the following libraries, which are developed in lockstep:

- [🌪️ WHIR](https://github.com/WizardOfMenlo/whir)
- [Spongefish](https://github.com/arkworks-rs/spongefish)
- [gnark-skyscraper](https://github.com/reilabs/gnark-skyscraper)
- [gnark-nimue](https://github.com/reilabs/gnark-nimue)
- [gnark-whir](https://github.com/reilabs/gnark-whir)
- [noir](https://github.com/noir-lang/noir)
