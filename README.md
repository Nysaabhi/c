# c
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Airdrop Search Engine</title>
    
    <!-- Load React -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.5/babel.min.js"></script>
    
    <!-- Load Tailwind CSS -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.js"></script>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        function AirdropSearchEngine() {
            const [searchTerm, setSearchTerm] = useState('');
            const [filters, setFilters] = useState({
                minValue: '',
                chain: 'all',
                status: 'all'
            });

            // Sample airdrop data
            const airdrops = [
                {
                    id: 1,
                    name: "Ethereum 2.0 Airdrop",
                    chain: "Ethereum",
                    value: "500",
                    status: "Active",
                    endDate: "2024-12-31",
                    requirements: "Must have staked ETH",
                    website: "https://ethereum.org"
                },
                {
                    id: 2,
                    name: "Arbitrum Token Drop",
                    chain: "Arbitrum",
                    value: "300",
                    status: "Upcoming",
                    endDate: "2024-11-30",
                    requirements: "Previous network usage required",
                    website: "https://arbitrum.io"
                },
                {
                    id: 3,
                    name: "Optimism Governance",
                    chain: "Optimism",
                    value: "400",
                    status: "Active",
                    endDate: "2024-12-15",
                    requirements: "Must have used Optimism",
                    website: "https://optimism.io"
                }
            ];

            const filteredAirdrops = airdrops.filter(airdrop => {
                const matchesSearch = airdrop.name.toLowerCase().includes(searchTerm.toLowerCase());
                const matchesValue = filters.minValue === '' || parseInt(airdrop.value) >= parseInt(filters.minValue);
                const matchesChain = filters.chain === 'all' || airdrop.chain === filters.chain;
                const matchesStatus = filters.status === 'all' || airdrop.status === filters.status;
                return matchesSearch && matchesValue && matchesChain && matchesStatus;
            });

            return (
                <div className="min-h-screen bg-gray-100 p-8">
                    <div className="max-w-6xl mx-auto">
                        <h1 className="text-4xl font-bold text-center mb-8 text-blue-600">
                            Crypto Airdrop Search
                        </h1>
                        
                        {/* Search and Filters */}
                        <div className="bg-white rounded-lg shadow-md p-6 mb-8">
                            <div className="mb-4">
                                <input
                                    type="text"
                                    placeholder="Search airdrops..."
                                    className="w-full p-3 border rounded-lg"
                                    value={searchTerm}
                                    onChange={(e) => setSearchTerm(e.target.value)}
                                />
                            </div>
                            
                            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <select
                                    className="p-2 border rounded-lg"
                                    value={filters.chain}
                                    onChange={(e) => setFilters({...filters, chain: e.target.value})}
                                >
                                    <option value="all">All Chains</option>
                                    <option value="Ethereum">Ethereum</option>
                                    <option value="Arbitrum">Arbitrum</option>
                                    <option value="Optimism">Optimism</option>
                                </select>
                                
                                <select
                                    className="p-2 border rounded-lg"
                                    value={filters.status}
                                    onChange={(e) => setFilters({...filters, status: e.target.value})}
                                >
                                    <option value="all">All Status</option>
                                    <option value="Active">Active</option>
                                    <option value="Upcoming">Upcoming</option>
                                    <option value="Ended">Ended</option>
                                </select>
                                
                                <input
                                    type="number"
                                    placeholder="Min. Value ($)"
                                    className="p-2 border rounded-lg"
                                    value={filters.minValue}
                                    onChange={(e) => setFilters({...filters, minValue: e.target.value})}
                                />
                            </div>
                        </div>
                        
                        {/* Results */}
                        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                            {filteredAirdrops.map(airdrop => (
                                <div key={airdrop.id} className="bg-white rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow">
                                    <div className="flex justify-between items-start mb-4">
                                        <h2 className="text-xl font-semibold text-gray-800">{airdrop.name}</h2>
                                        <span className={`px-3 py-1 rounded-full text-sm ${
                                            airdrop.status === 'Active' ? 'bg-green-100 text-green-800' :
                                            airdrop.status === 'Upcoming' ? 'bg-blue-100 text-blue-800' :
                                            'bg-gray-100 text-gray-800'
                                        }`}>
                                            {airdrop.status}
                                        </span>
                                    </div>
                                    
                                    <div className="space-y-2 text-gray-600">
                                        <p><span className="font-medium">Chain:</span> {airdrop.chain}</p>
                                        <p><span className="font-medium">Value:</span> ${airdrop.value}</p>
                                        <p><span className="font-medium">End Date:</span> {airdrop.endDate}</p>
                                        <p><span className="font-medium">Requirements:</span> {airdrop.requirements}</p>
                                    </div>
                                    
                                    <a
                                        href={airdrop.website}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className="mt-4 block w-full text-center bg-blue-500 text-white py-2 rounded-lg hover:bg-blue-600 transition-colors"
                                    >
                                        View Airdrop
                                    </a>
                                </div>
                            ))}
                        </div>
                        
                        {filteredAirdrops.length === 0 && (
                            <div className="text-center text-gray-500 mt-8">
                                No airdrops found matching your criteria
                            </div>
                        )}
                    </div>
                </div>
            );
        }

        // Render the app
        ReactDOM.render(<AirdropSearchEngine />, document.getElementById('root'));
    </script>
</body>
</html>
