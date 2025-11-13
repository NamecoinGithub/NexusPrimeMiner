# NexusPrimeMiner

**Standalone Nexus Prime Miner - Mines real Cunningham chains independently**

✅ Mines real Cunningham chains independently  
✅ Connects to Nexus Core on port 0 with auto-detect fallback  
✅ Shows real stats (no simulation)  
✅ Can run 24/7 as a daemon  
✅ Can be wrapped in GUI later  
✅ Solves the "blockchain caboose" problem - actually guides the blockchain forward  

## Key Features

1. **Port 0 Auto-detect**: Set port to "0" in config and the miner will automatically try standard ports (9325 for solo, 9549 for pool)
2. **Simple Solo Mining**: Use `miner.conf.solo-example` as a template for solo mining configuration
3. **Simple Pool Mining**: Use `miner.conf.pool-example` as a template for pool mining configuration
4. **Optimized Performance**: Advanced sieving algorithms and AVX2 optimizations for maximum chain discovery rate
5. **Multi-threaded**: Separate configurable thread pools for sieving and primality testing

## Build Instructions (Ubuntu/Linux)

### Install Dependencies

```sh
sudo apt-get install build-essential libboost-all-dev libdb-dev libdb++-dev libssl-dev libminiupnpc-dev libgmp-dev
```

### Clone the Repository

```sh
git clone https://github.com/NamecoinGithub/NexusPrimeMiner.git
cd NexusPrimeMiner
```

### Build the Miner

```sh
make MARCHFLAGS=-march=native -f makefile
```

On Windows with MinGW:
```sh
make -f makefile.mingw
```

## Configuration

### Using miner.conf

The miner can be configured via `miner.conf`. Example configuration files are included:
- `miner.conf.solo-example` - Template for solo mining
- `miner.conf.pool-example` - Template for pool mining

Copy the appropriate example to `miner.conf` and edit:

```sh
cp miner.conf.solo-example miner.conf
# Edit miner.conf with your settings
```

### Configuration Parameters

```json
{
  "host": "localhost",              // Pool/Node hostname or IP address
  "port": "0",                      // Port number (use "0" for auto-detect)
  "nxs_address": "",                // Your NXS payout address (empty for solo)
  "sieve_threads": 0,               // Sieving threads (0 = use all cores)
  "ptest_threads": 0,               // Prime test threads (0 = use all cores)
  "timeout": 10,                    // Connection timeout in seconds
  "bit_array_size": 8388608,        // Sieve array size in bytes
  "prime_limit": 7137857100,        // Maximum prime for sieve initialization
  "n_prime_limit": 81920,           // Maximum inverse prime limit
  "primorial_end_prime": 12,        // Largest primorial prime
  "experimental": "true"            // Use experimental optimizations
}
```

### Solo Mining Configuration

For solo mining, set `nxs_address` to an empty string (`""`):

```json
{
  "host": "localhost",
  "port": "9325",
  "nxs_address": "",
  "sieve_threads": 0,
  "ptest_threads": 0,
  "timeout": 10
}
```

Or use port auto-detect:
```json
{
  "host": "localhost",
  "port": "0",
  "nxs_address": ""
}
```

### Pool Mining Configuration

For pool mining, provide your NXS payout address:

```json
{
  "host": "nexusminingpool.com",
  "port": "9549",
  "nxs_address": "YOUR_NXS_ADDRESS_HERE",
  "sieve_threads": 0,
  "ptest_threads": 0,
  "timeout": 10
}
```

## Running the Miner

### Using Configuration File

Simply run the executable after creating `miner.conf`:

```sh
./nexus_cpuminer
```

### Using Command Line Arguments

You can also run with command line arguments (overrides config file):

**Solo Mining:**
```sh
./nexus_cpuminer localhost 9325 ""
```

**Pool Mining:**
```sh
./nexus_cpuminer nexusminingpool.com 9549 YOUR_NXS_ADDRESS
```

**With Thread Configuration:**
```sh
./nexus_cpuminer HOST PORT ADDRESS SIEVE_THREADS PTEST_THREADS TIMEOUT
```

### Running as a Daemon

On Linux, you can run the miner as a background daemon:

```sh
./soloMining.sh &
```

Or on Windows:
```cmd
soloMining.bat
```

For pool mining, edit `run_pool.bat` or create a similar shell script.

## Performance Tuning

### Optimize for Your CPU

- **bit_array_size**: Adjust to fit your CPU L3 cache (default: 8388608 bytes = 8MB)
- **sieve_threads**: Set to number of physical cores for best results
- **ptest_threads**: Can be set higher than physical cores (1.5-2x)
- **MARCHFLAGS**: Use `-march=native` when building for maximum optimization

### Monitor Performance

The miner displays real-time statistics:
- **PPS**: Primes Per Second
- **CPS**: Chains Per Second (for chains above difficulty 3.x)
- **WPS**: Weighted Prime Shares per Second
- **Difficulty**: Current network difficulty

## What are Cunningham Chains?

Cunningham chains are sequences of prime numbers where each prime is related to the next. The Nexus network uses these chains for proof-of-work, specifically:

- **First Kind (1CC)**: p, 2p+1, 4p+3, 8p+7, ...
- **Second Kind (2CC)**: p, 2p-1, 4p-3, 8p-7, ...

This miner efficiently discovers these chains using advanced sieving and primality testing algorithms.

## Advanced Features

### Experimental Sieve

Set `"experimental": "true"` to enable optimized sieving algorithms with AVX2 support (if your CPU supports it).

### OpenACC GPU Acceleration (Experimental)

If compiled with PGI/NVIDIA compilers, GPU acceleration can be used for sieving:

```sh
# Build with OpenACC support
pgc++ -acc -ta=nvidia -o nexus_cpuminer_gpu miner.cpp ...
```

## Troubleshooting

### Connection Issues

- Verify your Nexus Core node is running and RPC is enabled
- Check firewall settings allow connections on the specified port
- For solo mining, ensure port 9325 (mainnet) or 8325 (testnet) is correct
- Try port "0" for auto-detection if you're unsure of the correct port

### Low Hash Rate

- Ensure `bit_array_size` fits in your CPU cache
- Compile with `-march=native` for CPU-specific optimizations
- Adjust thread counts - more isn't always better
- Close other CPU-intensive applications

### Build Errors

- Verify all dependencies are installed
- On older systems, you may need to specify boost library paths
- For GMP errors, ensure libgmp-dev is installed

## Contributing

This project merges features from:
- [PrimeSoloMiner](https://github.com/NamecoinGithub/PrimeSoloMiner)
- [PrimePoolMiner](https://github.com/NamecoinGithub/PrimePoolMiner)

Contributions are welcome! Please submit pull requests or open issues for bugs and feature requests.

## Credits

Created by Videlicet  
Optimized by Supercomputing, paulscreen, hashtobewild & mumus  
Merged and enhanced by the Nexus community

## License

This software is provided as-is for mining on the Nexus network.
