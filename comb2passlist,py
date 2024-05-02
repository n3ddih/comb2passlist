import argparse
import json
import requests

def get_passwords(username, proxy=None) -> list:
    # Pad the username with '@' if length < 4
    if len(username) < 4:
        username += '@' * (4 - len(username))
    
    url = f"https://api.proxynova.com/comb?query={username}"
    response = requests.get(url, proxies={"http": proxy, "https": proxy} if proxy else None)
    response.raise_for_status()

    data = response.json()
    passwords = [line.split(":")[1] if ":" in line else line.split(";")[1] for line in data["lines"]]
    return list(set(passwords))

def main() -> None:
    parser = argparse.ArgumentParser(description="Get passwords for a given username or list of usernames.")
    parser.add_argument("-u", "--username", help="Username to query.")
    parser.add_argument("-l", "--username-list", help="File containing a list of usernames to query.")
    parser.add_argument("-p", "--proxy", help="Proxy to use for requests.")
    parser.add_argument("-o", "--output", help="Output file name.")
    parser.add_argument("--output-format", choices=["list", "dict"], default="list", help="Output format (list or dict).")

    args = parser.parse_args()

    if not (args.username or args.username_list):
        parser.error("At least one of -u/--username or -l/--username-list must be provided.")

    if args.username and args.username_list:
        parser.error("Only one of -u/--username or -l/--username-list can be provided.")

    if args.username:
        passwords = get_passwords(args.username, args.proxy)
        output = [{args.username: passwords}]
    else:
        with open(args.username_list, "r") as f:
            usernames = [line.strip() for line in f]

        output = []
        for username in usernames:
            passwords = get_passwords(username, args.proxy)
            output.append({username: passwords})

    if args.output_format == "list":
        output_str = ["\n".join(passwords) for items in output for passwords in items.values()]
        output_str = "\n".join(output_str)
    elif args.output_format == "dict":
        output_str = json.dumps(output, indent=2)
    
    print(output_str)    
    if args.output:
        with open(args.output, "w") as f:
            f.write(output_str)
        

if __name__ == "__main__":
    main()
