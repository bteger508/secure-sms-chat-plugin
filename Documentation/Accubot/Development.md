# Development Documentation

## Setting up the dev environment

This documentation assumes you already have the secure chat system development environment
setup. If not, refer to the [secure chat development documentation](../Development.md).

**Note: the system currently only works when running in a Docker container. If you run the system
normally (without docker), the system can't find the training and responses data files.**

### Getting an API Key from Finage

- Register for an account with [Finage](https://finage.co.uk)
- Select the API plan that best fits your needs
- From the API dashboard, copy your API Key
- Replace the API key in `MarketDataService.cs` with your own key
