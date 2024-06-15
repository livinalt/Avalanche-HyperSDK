
# TokenVM Project

The TokenVM project enables the creation of a custom blockchain with full control over its rules and functionality. This includes minting and transferring tokens and managing order books for asset trading.

## Prerequisites

- **Go**: Ensure you have Go installed on your system. [Download Go](https://golang.org/dl/) and follow the installation instructions for your operating system.

## Installation

1. **Clone the Repository**
    ```bash
    git clone https://github.com/livinalt/Avalanche-HyperSDK
    cd tokenvm
    ```

2. **Normalize Dependencies**
    ```bash
    go mod tidy
    ```


## Running the VM

1. **Set Go Path**
    - Ensure Go is on your path:
        ```bash
        export PATH=$PATH:$(go env GOPATH)/bin
        ```
    - Alternatively:
        ```bash
        export PATH=$PATH:/usr/local/go/bin
        ```

2. **Run the VM Locally**
    ```bash
    MODE="run-single" ./scripts/run.sh
    ```

3. **Build the Project**
    ```bash
    ./scripts/build.sh
    ```
    - If you encounter a permissions error, use:
        ```bash
        bash ./scripts/run.sh
        bash ./scripts/build.sh
        ```

## Interacting with the VM

1. **Import the Demo Private Key**
    ```bash
    ./build/token-cli key import demo.pk
    ```

2. **Import the Chain**
    ```bash
    ./build/token-cli chain import-anr
    ```

3. **Use the Demos**
    - Follow the demos in the README file or refer to the repository's documentation.

## Troubleshooting

- **Common Errors**
    - `No such file or directory`: Ensure you have built the project successfully and the path is correct.


## Demos


### Launch Your Own TokenVM Subnet

The first step to running these demos is to launch your own tokenvm Subnet. Run the following command (may take a few minutes):

```sh
./scripts/run.sh
```

By default, this allocates all funds on the network to `token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp`. The private key for this address is `0x323b1d8f4eed5f0da9da93071b034f2dce9d2d22692c172f3cb252a64ddfafd01b057de320297c29ad0c1f589ea216869cf1938d88c9fbd70d6748323dbf2fa7`. For convenience, this key is also stored at `demo.pk`.

If you don't need 2 Subnets for your testing, run:

```sh
MODE="run-single" ./scripts/run.sh
```

### Build the Token CLI

To make it easy to interact with the tokenvm, we implemented the `token-cli`. Build it using the following command:

```sh
./scripts/build.sh
```

This command will put the compiled CLI in `./build/token-cli`.

### Import Chains and Keys

Add the chains you created and the default key to the `token-cli` using the following commands:

```sh
./build/token-cli key import demo.pk
./build/token-cli chain import-anr
```

`chain import-anr` connects to the Avalanche Network Runner server running in the background and pulls the URIs of all nodes tracking each chain you created.

## Mint and Trade

### Step 1: Create Your Asset

Create your own asset with the following command:

```sh
./build/token-cli action create-asset
```

Expected output:

```plaintext
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
metadata (can be changed later): MarioCoin
continue (y/n): y
✅ txID: 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
```

`txID` is the `assetID` of your new asset.

### Step 2: Mint Your Asset

Mint some of your asset:

```sh
./build/token-cli action mint-asset
```

Expected output:

```plaintext
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
assetID: 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 0
recipient: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
amount: 10000
continue (y/n): y
✅ txID: X1E5CVFgFFgniFyWcj5wweGg66TyzjK2bMWWTzFwJcwFYkF72
```

### Step 3: View Your Balance

Check your balance:

```sh
./build/token-cli key balance
```

Expected output:

```plaintext
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 10000
warp: false
balance: 10000 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
```

### Step 4: Create an Order

Create an order on-chain to trade your token:

```sh
./build/token-cli action create-order
```

Expected output:

```plaintext
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
in assetID (use TKN for native token): TKN
✔ in tick: 1█
out assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 10000
warp: false
balance: 10000 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
out tick: 10
supply (must be multiple of out tick): 100
continue (y/n): y
✅ txID: 2TdeT2ZsQtJhbWJuhLZ3eexuCY4UP6W7q5ZiAHMYtVfSSp1ids
```

### Step 5: Fill Part of the Order

Fill your order:

```sh
./build/token-cli action fill-order
```

Expected output:

```plaintext
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
in assetID (use TKN for native token): TKN
balance: 997.999993843 TKN
out assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 10000
warp: false
available orders: 1
0) Rate(in/out): 100000000.0000
   InTick: 1.000000000 TKN
   OutTick: 10 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
   Remaining: 100 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
select order: 0
value (must be multiple of in tick): 2
in: 2.000000000 TKN
out: 20 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
continue (y/n): y
✅ txID: uw9YrZcs4QQTEBSR3guVnzQTFyKKm5QFGVTvuGyntSTrx3aGm
```

### Step 6: Close Order

Cancel your order:

```sh
./build/token-cli action close-order
```

Expected output:

```plaintext
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
orderID: 2TdeT2ZsQtJhbWJuhLZ3eexuCY4UP6W7q5ZiAHMYtVfSSp1ids
out assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
continue (y/n): y
✅ txID: poGnxYiLZAruurNjugTPfN1JjwSZzGZdZn

Q24fnDMPvxtgWrT
```

## Author
Jeremiah Samuel - livinalt@gmail.com

## Contributing

If you want to make contributions, feel free to clone the repo and proceed


## License

This project is licensed under the BSD-3-Clause license.
