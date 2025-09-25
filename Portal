<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Employee Wellness Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            color: #1E3743;
        }
        .text-\[\#C5076C\] {
            color: #C5076C;
        }
        .bg-\[\#C3E9E7\] {
            background-color: #C3E9E7;
        }
        .bg-\[\#FEC58F\] {
            background-color: #FEC58F;
        }
        .bg-\[\#fab56e\] {
            background-color: #fab56e;
        }
        .text-\[\#1E3743\] {
            color: #1E3743;
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useCallback } = React;

        // --- DEVELOPMENT FLAG ---
        const SKIP_LOGIN = true;
        const START_AS_ADMIN = true;

        const MOCK_USER = { uid: 'emp123', email: 'employee@example.com', displayName: 'Jane Doe' };
        const MOCK_ADMIN = { uid: 'admin456', email: 'payroll@example.com', displayName: 'Admin User' };
        
        // Mock Firebase for initial display if not connected
        // All Firebase logic is commented out to allow the file to run standalone.
        // It relies on mock data and console logs to simulate behavior.
        const auth = null;
        const db = null;
        const storage = null;
        const functions = null;

        // --- UTILITY & HELPER COMPONENTS ---
        const Spinner = ({ color = 'white' }) => (
            <div className={`animate-spin rounded-full h-5 w-5 border-b-2 border-${color}`}></div>
        );

        const Alert = ({ message, type, onClose }) => {
            const baseClasses = 'p-4 mb-4 text-sm rounded-lg relative';
            const typeClasses = {
                success: 'bg-green-100 text-green-700',
                error: 'bg-red-100 text-red-700',
                info: 'bg-blue-100 text-blue-700',
            };
            return (
                <div className={`${baseClasses} ${typeClasses[type]}`} role="alert">
                    <span className="font-medium">{message}</span>
                    <button onClick={onClose} className="absolute top-0 right-0 mt-2 mr-3 text-lg font-bold">&times;</button>
                </div>
            );
        };

        const StatusBadge = ({ status }) => {
            const statusStyles = {
                Pending: 'bg-yellow-200 text-yellow-800',
                Approved: 'bg-[#C3E9E7] text-gray-800',
                Rejected: 'bg-red-200 text-red-800',
                'Awaiting Payout': 'bg-[#C3E9E7] text-gray-800',
                Paid: 'bg-[#C3E9E7] text-gray-800',
            };
            return (
                <span className={`px-3 py-1 text-xs font-semibold rounded-full ${statusStyles[status] || 'bg-gray-200 text-gray-800'}`}>
                    {status}
                </span>
            );
        };


        // --- AUTHENTICATION COMPONENT ---
        const AuthScreen = ({ setUser }) => {
            const [isLogin, setIsLogin] = useState(true);
            const [email, setEmail] = useState('');
            const [password, setPassword] = useState('');
            const [name, setName] = useState('');
            const [error, setError] = useState('');
            const [loading, setLoading] = useState(false);

            const handleAuthAction = async (e) => {
                e.preventDefault();
                setLoading(true);
                setError('');
                try {
                    // Mock auth action
                    await new Promise(resolve => setTimeout(resolve, 1000));
                    if (email === 'admin@example.com' && password === 'password') {
                        setUser(MOCK_ADMIN);
                    } else if (email === 'employee@example.com' && password === 'password') {
                        setUser(MOCK_USER);
                    } else {
                        throw new Error("Invalid credentials. Try employee@example.com or admin@example.com with password 'password'.");
                    }
                } catch (err) {
                    setError(err.message);
                } finally {
                    setLoading(false);
                }
            };

            return (
                <div className="w-full max-w-md p-8 space-y-6 bg-white rounded-lg shadow-md">
                    <div>
                        <h2 className="text-2xl font-bold text-center">{isLogin ? 'Sign In' : 'Create Account'}</h2>
                        <p className="text-center text-gray-600">to access the Wellness Portal</p>
                    </div>
                    {error && <Alert message={error} type="error" onClose={() => setError('')} />}
                    <form className="space-y-4" onSubmit={handleAuthAction}>
                        {!isLogin && (
                            <div>
                                <label className="block text-sm font-medium">Full Name</label>
                                <input type="text" value={name} onChange={(e) => setName(e.target.value)} required className="w-full px-3 py-2 mt-1 border rounded-md" />
                            </div>
                        )}
                        <div>
                            <label className="block text-sm font-medium">Email</label>
                            <input type="email" value={email} onChange={(e) => setEmail(e.target.value)} required className="w-full px-3 py-2 mt-1 border rounded-md" />
                        </div>
                        <div>
                            <label className="block text-sm font-medium">Password</label>
                            <input type="password" value={password} onChange={(e) => setPassword(e.target.value)} required className="w-full px-3 py-2 mt-1 border rounded-md" />
                        </div>
                        <button type="submit" disabled={loading} className="w-full px-4 py-2 font-bold text-white bg-indigo-600 rounded-md hover:bg-indigo-700 disabled:bg-indigo-400 flex items-center justify-center">
                            {loading ? <Spinner /> : (isLogin ? 'Sign In' : 'Sign Up')}
                        </button>
                    </form>
                    <div className="text-center">
                        <button onClick={() => setIsLogin(!isLogin)} className="text-sm text-indigo-600 hover:underline">
                            {isLogin ? "Don't have an account? Sign Up" : "Already have an account? Sign In"}
                        </button>
                    </div>
                </div>
            );
        };


        // --- MAIN UI COMPONENTS ---
        const SubmissionForm = ({ user, setSubmissions }) => {
            const [formData, setFormData] = useState({
                purchaseDate: new Date().toISOString().split('T')[0],
                category: 'Gym Membership',
                description: '',
                amount: '',
            });
            const [receiptFile, setReceiptFile] = useState(null);
            const [isLoading, setIsLoading] = useState(false);
            const [alert, setAlert] = useState(null);

            const handleInputChange = (e) => {
                const { name, value } = e.target;
                setFormData(prev => ({ ...prev, [name]: value }));
            };

            const handleFileChange = (e) => {
                if (e.target.files[0]) {
                    setReceiptFile(e.target.files[0]);
                }
            };

            const handleSubmit = async (e) => {
                e.preventDefault();
                if (!receiptFile) {
                    setAlert({ type: 'error', message: 'Please upload a receipt.' });
                    return;
                }
                setIsLoading(true);
                setAlert(null);
                
                // Mock logic for demonstration without backend
                console.log("Submitting:", { ...formData, employeeName: user.displayName, employeeEmail: user.email });
                await new Promise(resolve => setTimeout(resolve, 1500));
                
                setAlert({ type: 'success', message: 'Expense submitted successfully! It is now pending review.' });
                e.target.reset();
                setFormData({
                    purchaseDate: new Date().toISOString().split('T')[0],
                    category: 'Gym Membership',
                    description: '',
                    amount: '',
                });
                setReceiptFile(null);

                setIsLoading(false);
            };

            return (
                <div className="bg-white p-8 rounded-2xl shadow-lg w-full max-w-2xl">
                    <h2 className="text-2xl font-bold text-gray-800 mb-6">Submit a Wellness Expense</h2>
                    {alert && <Alert message={alert.message} type={alert.type} onClose={() => setAlert(null)} />}
                    <form onSubmit={handleSubmit} className="space-y-4">
                        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label htmlFor="employeeName" className="block text-sm font-medium text-gray-700">Employee Name</label>
                                <input type="text" id="employeeName" value={user.displayName || 'N/A'} disabled className="mt-1 block w-full bg-gray-100 border-gray-300 rounded-md shadow-sm p-2" />
                            </div>
                            <div>
                                <label htmlFor="employeeEmail" className="block text-sm font-medium text-gray-700">Employee Email</label>
                                <input type="email" id="employeeEmail" value={user.email || 'N/A'} disabled className="mt-1 block w-full bg-gray-100 border-gray-300 rounded-md shadow-sm p-2" />
                            </div>
                        </div>
                        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label htmlFor="purchaseDate" className="block text-sm font-medium text-gray-700">Date of Purchase</label>
                                <input type="date" id="purchaseDate" name="purchaseDate" value={formData.purchaseDate} onChange={handleInputChange} required className="mt-1 block w-full border-gray-300 rounded-md shadow-sm p-2 focus:ring-indigo-500 focus:border-indigo-500" />
                            </div>
                            <div>
                                <label htmlFor="category" className="block text-sm font-medium text-gray-700">Category</label>
                                <select id="category" name="category" value={formData.category} onChange={handleInputChange} required className="mt-1 block w-full border-gray-300 rounded-md shadow-sm p-2 focus:ring-indigo-500 focus:border-indigo-500">
                                    <option>Gym Membership</option>
                                    <option>Fitness Class</option>
                                    <option>Exercise Equipment</option>
                                    <option>Wellness App</option>
                                    <option>Other</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label htmlFor="description" className="block text-sm font-medium text-gray-700">Description of Purchase</label>
                            <input type="text" id="description" name="description" value={formData.description} onChange={handleInputChange} required className="mt-1 block w-full border-gray-300 rounded-md shadow-sm p-2 focus:ring-indigo-500 focus:border-indigo-500" placeholder="e.g., Annual membership at Planet Fitness" />
                        </div>
                        <div>
                            <label htmlFor="amount" className="block text-sm font-medium text-gray-700">Amount ($)</label>
                            <input type="number" id="amount" name="amount" value={formData.amount} onChange={handleInputChange} required step="0.01" min="0" className="mt-1 block w-full border-gray-300 rounded-md shadow-sm p-2 focus:ring-indigo-500 focus:border-indigo-500" placeholder="59.99" />
                        </div>
                        <div>
                            <label htmlFor="receipt" className="block text-sm font-medium text-gray-700">Receipt Upload (Image or PDF)</label>
                            <input type="file" id="receipt" onChange={handleFileChange} required accept="image/*,.pdf" className="mt-1 block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-[#1E3743] hover:file:bg-indigo-100" />
                        </div>
                        <button type="submit" disabled={isLoading} className="w-full bg-[#FEC58F] text-[#1E3743] font-bold py-3 px-4 rounded-lg hover:bg-[#fab56e] focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-[#FEC58F] disabled:bg-gray-400 flex items-center justify-center">
                            {isLoading ? <Spinner color="[#1E3743]" /> : 'Submit Expense'}
                        </button>
                    </form>
                </div>
            );
        };

        const EmployeeDashboard = ({ user }) => {
            const [userData, setUserData] = useState({ currentBalance: 0, totalAccruedYTD: 0 });
            const [submissions, setSubmissions] = useState([]);
            const [loading, setLoading] = useState(true);

            useEffect(() => {
                if (!user) return;
                setLoading(true);
                
                // Mock data for display
                const MOCK_USER_DATA = { name: 'Jane Doe', email: 'employee@example.com', currentBalance: 150.50, totalAccruedYTD: 833.30 };
                const MOCK_SUBMISSIONS = [
                    { id: 'sub1', purchaseDate: { toDate: () => new Date('2025-09-15') }, description: 'Yoga Class Pass', amount: 75.00, status: 'Approved' },
                    { id: 'sub2', purchaseDate: { toDate: () => new Date('2025-09-02') }, description: 'Gym Membership - Sept', amount: 55.00, status: 'Awaiting Payout' },
                    { id: 'sub4', purchaseDate: { toDate: () => new Date('2025-09-10') }, description: 'Concert Tickets', amount: 95.00, status: 'Rejected', statusReason: 'Not an approved wellness expense' },
                    { id: 'sub3', purchaseDate: { toDate: () => new Date('2025-08-20') }, description: 'Running Shoes', amount: 120.00, status: 'Paid' },
                ];
                setUserData(MOCK_USER_DATA);
                setSubmissions(MOCK_SUBMISSIONS);
                setLoading(false);

            }, [user]);

            if (loading) {
                return <div className="text-center p-10">Loading Dashboard...</div>;
            }
            
            const totalReimbursed = submissions
                .filter(s => ['Awaiting Payout', 'Paid'].includes(s.status))
                .reduce((acc, sub) => acc + sub.amount, 0);

            return (
                <div className="space-y-8 w-full">
                    {/* Balance Cards */}
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div className="bg-white p-6 rounded-2xl shadow">
                            <h3 className="text-sm font-medium text-gray-500">Current Available Balance</h3>
                            <p className="mt-2 text-4xl font-bold text-[#C5076C]">${(userData.currentBalance || 0).toFixed(2)}</p>
                        </div>
                        <div className="bg-white p-6 rounded-2xl shadow">
                            <h3 className="text-sm font-medium text-gray-500">Total Accrued (YTD)</h3>
                            <p className="mt-2 text-3xl font-bold text-gray-800">${(userData.totalAccruedYTD || 0).toFixed(2)}</p>
                        </div>
                        <div className="bg-white p-6 rounded-2xl shadow">
                            <h3 className="text-sm font-medium text-gray-500">Total Reimbursed (YTD)</h3>
                            <p className="mt-2 text-3xl font-bold text-gray-800">${totalReimbursed.toFixed(2)}</p>
                        </div>
                    </div>

                    {/* Submission Form */}
                    <SubmissionForm user={user} setSubmissions={setSubmissions} />

                    {/* Submission History */}
                    <div className="bg-white p-8 rounded-2xl shadow-lg w-full">
                        <h2 className="text-2xl font-bold text-gray-800 mb-6">Submission History</h2>
                        <div className="overflow-x-auto">
                            <table className="w-full text-sm text-left text-gray-500">
                                <thead className="text-xs text-gray-700 uppercase bg-gray-50">
                                    <tr>
                                        <th scope="col" className="px-6 py-3">Date</th>
                                        <th scope="col" className="px-6 py-3">Description</th>
                                        <th scope="col" className="px-6 py-3">Amount</th>
                                        <th scope="col" className="px-6 py-3">Status</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    {submissions.map(sub => (
                                        <tr key={sub.id} className="bg-white border-b hover:bg-gray-50">
                                            <td className="px-6 py-4">{sub.purchaseDate.toDate().toLocaleDateString()}</td>
                                            <td className="px-6 py-4 font-medium text-gray-900">
                                                {sub.description}
                                                {sub.status === 'Rejected' && sub.statusReason && (
                                                    <p className="text-xs text-red-600 mt-1">Reason: {sub.statusReason}</p>
                                                )}
                                            </td>
                                            <td className="px-6 py-4">${sub.amount.toFixed(2)}</td>
                                            <td className="px-6 py-4"><StatusBadge status={sub.status} /></td>
                                        </tr>
                                    ))}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            );
        };

        const ReceiptModal = ({ isOpen, onClose, imageUrl }) => {
            if (!isOpen) return null;

            return (
                <div className="fixed inset-0 bg-black bg-opacity-75 flex justify-center items-center z-50" onClick={onClose}>
                    <div className="bg-white p-4 rounded-lg shadow-xl max-w-lg max-h-[90vh] relative" onClick={e => e.stopPropagation()}>
                        <button onClick={onClose} className="absolute -top-2 -right-2 bg-white rounded-full p-1 text-2xl font-bold">&times;</button>
                        <img src={imageUrl} alt="Receipt" className="max-w-full max-h-[85vh] object-contain" />
                    </div>
                </div>
            );
        };

        const RejectionModal = ({ isOpen, onClose, onSubmit, submission }) => {
            const [reason, setReason] = useState('Not an approved wellness expense');

            if (!isOpen || !submission) return null;

            const handleSubmit = () => {
                onSubmit(submission.id, 'Rejected', reason);
            };

            return (
                <div className="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50">
                    <div className="bg-white p-6 rounded-lg shadow-xl w-full max-w-sm">
                        <h3 className="text-lg font-bold mb-4">Reject Submission</h3>
                        <p className="text-sm mb-2">You are about to reject the following submission:</p>
                        <div className="bg-gray-100 p-3 rounded-md mb-4">
                            <p className="font-semibold">{submission.description}</p>
                            <p className="text-sm text-gray-600">${submission.amount.toFixed(2)} - {submission.employeeName}</p>
                        </div>
                        <label htmlFor="rejectionReason" className="block text-sm font-medium text-gray-700">Reason for Rejection</label>
                        <select
                            id="rejectionReason"
                            value={reason}
                            onChange={(e) => setReason(e.target.value)}
                            className="mt-1 block w-full border-gray-300 rounded-md shadow-sm p-2 focus:ring-indigo-500 focus:border-indigo-500"
                        >
                            <option>Not an approved wellness expense</option>
                            <option>Receipt unclear or invalid</option>
                            <option>Duplicate submission</option>
                            <option>Other</option>
                        </select>
                        <div className="mt-6 flex justify-end space-x-3">
                            <button onClick={onClose} className="px-4 py-2 text-sm font-medium text-gray-700 bg-gray-200 rounded-md hover:bg-gray-300">
                                Cancel
                            </button>
                            <button onClick={handleSubmit} className="px-4 py-2 text-sm font-medium text-white bg-red-600 rounded-md hover:bg-red-700">
                                Confirm Rejection
                            </button>
                        </div>
                    </div>
                </div>
            );
        };

        const PendingReview = ({ pendingItems, onUpdateStatus }) => {
            const [rejectionModalOpen, setRejectionModalOpen] = useState(false);
            const [selectedItemForRejection, setSelectedItemForRejection] = useState(null);
            const [receiptModalOpen, setReceiptModalOpen] = useState(false);
            const [selectedReceiptUrl, setSelectedReceiptUrl] = useState('');

            const handleRejectClick = (item) => {
                setSelectedItemForRejection(item);
                setRejectionModalOpen(true);
            };
            
            const handleViewReceiptClick = (url) => {
                setSelectedReceiptUrl(url);
                setReceiptModalOpen(true);
            };

            const handleConfirmRejection = (submissionId, status, reason) => {
                onUpdateStatus(submissionId, status, reason);
                setRejectionModalOpen(false);
                setSelectedItemForRejection(null);
            };

            if (pendingItems.length === 0) return null;

            return (
                <>
                    <RejectionModal
                        isOpen={rejectionModalOpen}
                        onClose={() => setRejectionModalOpen(false)}
                        onSubmit={handleConfirmRejection}
                        submission={selectedItemForRejection}
                    />
                    <ReceiptModal 
                        isOpen={receiptModalOpen}
                        onClose={() => setReceiptModalOpen(false)}
                        imageUrl={selectedReceiptUrl}
                    />
                    <div className="bg-white p-8 rounded-2xl shadow-lg w-full">
                        <h2 className="text-2xl font-bold text-gray-800 mb-2">Pending Submissions for Review</h2>
                        <p className="text-gray-600 mb-6">Approve or reject new submissions from employees.</p>
                        <div className="overflow-x-auto">
                            <table className="w-full text-sm text-left text-gray-500">
                                <thead className="text-xs text-gray-700 uppercase bg-gray-50">
                                    <tr>
                                        <th className="px-6 py-3">Date</th>
                                        <th className="px-6 py-3">Employee</th>
                                        <th className="px-6 py-3">Description</th>
                                        <th className="px-6 py-3">Amount</th>
                                        <th className="px-6 py-3">Receipt</th>
                                        <th className="px-6 py-3">Actions</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    {pendingItems.map(item => (
                                        <tr key={item.id} className="bg-white border-b hover:bg-gray-50">
                                            <td className="px-6 py-4">{item.purchaseDate.toDate().toLocaleDateString()}</td>
                                            <td className="px-6 py-4 font-medium text-gray-900">{item.employeeName}</td>
                                            <td className="px-6 py-4">{item.description}</td>
                                            <td className="px-6 py-4 font-bold">${item.amount.toFixed(2)}</td>
                                            <td className="px-6 py-4">
                                                <button onClick={() => handleViewReceiptClick(item.receiptUrl)} className="text-[#1E3743] hover:underline text-xs">View</button>
                                            </td>
                                            <td className="px-6 py-4 flex space-x-2">
                                                <button onClick={() => onUpdateStatus(item.id, 'Approved')} className="px-3 py-1 text-xs font-medium text-white bg-green-600 rounded-md hover:bg-green-700">Approve</button>
                                                <button onClick={() => handleRejectClick(item)} className="px-3 py-1 text-xs font-medium text-white bg-red-600 rounded-md hover:bg-red-700">Reject</button>
                                            </td>
                                        </tr>
                                    ))}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </>
            );
        };

        const EmployeeUploader = () => {
            const [file, setFile] = useState(null);
            const [isLoading, setIsLoading] = useState(false);
            const [alert, setAlert] = useState(null);

            const handleFileChange = (e) => {
                setFile(e.target.files[0]);
            };

            const handleUpload = () => {
                if (!file) {
                    setAlert({ type: 'error', message: 'Please select a CSV file to upload.'});
                    return;
                }
                setIsLoading(true);
                setAlert(null);

                // Papa is loaded from CDN, available on the window object
                window.Papa.parse(file, {
                    header: true,
                    skipEmptyLines: true,
                    complete: async (results) => {
                        const employees = results.data;
                        console.log('Parsed employees:', employees);
                        await new Promise(res => setTimeout(res, 1500));
                        setAlert({type: 'success', message: `Successfully processed ${employees.length} employees.`});
                        setIsLoading(false);
                    },
                    error: (error) => {
                        setAlert({ type: 'error', message: 'Failed to parse CSV file.' });
                        setIsLoading(false);
                    }
                });
            };

            return (
                <div className="bg-white p-8 rounded-2xl shadow-lg w-full">
                    <h2 className="text-2xl font-bold text-gray-800 mb-2">Bulk Employee Upload</h2>
                    <p className="text-gray-600 mb-4">Upload a CSV file with employee details. The file must contain "Name", "Email", and "Associate ID" columns.</p>
                    {alert && <Alert message={alert.message} type={alert.type} onClose={() => setAlert(null)} />}
                    <div className="flex items-center space-x-4">
                        <input 
                            type="file" 
                            accept=".csv"
                            onChange={handleFileChange}
                            className="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-[#1E3743] hover:file:bg-indigo-100"
                        />
                        <button onClick={handleUpload} disabled={isLoading} className="bg-[#FEC58F] text-[#1E3743] font-bold py-2 px-6 rounded-lg hover:bg-[#fab56e] flex items-center justify-center min-w-[120px]">
                            {isLoading ? <Spinner color="[#1E3743]" /> : 'Upload'}
                        </button>
                    </div>
                </div>
            );
        };

        const EmployeeLookup = () => {
            const [searchTerm, setSearchTerm] = useState('');
            const [isLoading, setIsLoading] = useState(false);
            const [searchResults, setSearchResults] = useState(null);
            const [error, setError] = useState('');

            const handleSearch = async (e) => {
                e.preventDefault();
                if (!searchTerm.trim()) return;
                setIsLoading(true);
                setError('');
                setSearchResults(null);
                
                // Mock Search
                await new Promise(res => setTimeout(res, 1000));
                if (searchTerm.toLowerCase().includes('jane')) {
                    setSearchResults({
                        user: { name: 'Jane Doe', email: 'employee@example.com', associateId: 'EMP-12345', currentBalance: 150.50, totalAccruedYTD: 833.30 },
                        submissions: [
                            { id: 'sub1', purchaseDate: { toDate: () => new Date('2025-09-15') }, description: 'Yoga Class Pass', amount: 75.00, status: 'Approved' },
                            { id: 'sub2', purchaseDate: { toDate: () => new Date('2025-09-02') }, description: 'Gym Membership - Sept', amount: 55.00, status: 'Awaiting Payout' },
                        ]
                    });
                } else {
                    setError('No employee found with that criteria.');
                }
                setIsLoading(false);
            };

            return (
                <div className="bg-white p-8 rounded-2xl shadow-lg w-full">
                    <h2 className="text-2xl font-bold text-gray-800 mb-2">Employee Lookup</h2>
                    <p className="text-gray-600 mb-4">Search for an employee by name, email, or Associate ID to view their submissions and totals.</p>
                    <form onSubmit={handleSearch} className="flex items-center space-x-4 mb-6">
                        <input
                            type="text"
                            value={searchTerm}
                            onChange={(e) => setSearchTerm(e.target.value)}
                            placeholder="Search..."
                            className="w-full px-3 py-2 border rounded-md"
                        />
                        <button type="submit" disabled={isLoading} className="bg-[#1E3743] text-white font-bold py-2 px-6 rounded-lg hover:bg-gray-700 flex items-center justify-center min-w-[120px]">
                            {isLoading ? <Spinner /> : 'Search'}
                        </button>
                    </form>
                    
                    {error && <Alert message={error} type="error" onClose={() => setAlert(null)} />}

                    {searchResults && (
                        <div>
                            <h3 className="text-xl font-bold mb-4">{searchResults.user.name}</h3>
                            <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                                <div className="bg-gray-100 p-4 rounded-lg">
                                    <h4 className="text-sm font-medium text-gray-500">Current Balance</h4>
                                    <p className="text-2xl font-bold">${searchResults.user.currentBalance.toFixed(2)}</p>
                                </div>
                                <div className="bg-gray-100 p-4 rounded-lg">
                                    <h4 className="text-sm font-medium text-gray-500">Total Accrued (YTD)</h4>
                                    <p className="text-2xl font-bold">${searchResults.user.totalAccruedYTD.toFixed(2)}</p>
                                </div>
                                <div className="bg-gray-100 p-4 rounded-lg">
                                    <h4 className="text-sm font-medium text-gray-500">Associate ID</h4>
                                    <p className="text-lg font-bold">{searchResults.user.associateId}</p>
                                </div>
                            </div>

                            <h4 className="text-lg font-semibold mb-2">Submission History</h4>
                            <div className="overflow-x-auto border rounded-lg">
                                <table className="w-full text-sm text-left text-gray-500">
                                    <thead className="text-xs text-gray-700 uppercase bg-gray-50">
                                        <tr>
                                            <th className="px-6 py-3">Date</th>
                                            <th className="px-6 py-3">Description</th>
                                            <th className="px-6 py-3">Amount</th>
                                            <th className="px-6 py-3">Status</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {searchResults.submissions.map(sub => (
                                            <tr key={sub.id} className="bg-white border-b hover:bg-gray-50">
                                                <td className="px-6 py-4">{sub.purchaseDate.toDate().toLocaleDateString()}</td>
                                                <td className="px-6 py-4">{sub.description}</td>
                                                <td className="px-6 py-4">${sub.amount.toFixed(2)}</td>
                                                <td className="px-6 py-4"><StatusBadge status={sub.status} /></td>
                                            </tr>
                                        ))}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    )}
                </div>
            );
        };


        const AdminDashboard = () => {
            const [payoutItems, setPayoutItems] = useState([]);
            const [pendingItems, setPendingItems] = useState([]);
            const [loading, setLoading] = useState(true);
            const [generating, setGenerating] = useState(false);

            useEffect(() => {
                setLoading(true);
                // Mock data for display
                const MOCK_ADMIN_PAYOUT_LIST = [
                    { id: 'sub2', employeeName: 'Jane Doe', email: 'employee@example.com', amount: 55.00, description: 'Gym Membership - Sept', purchaseDate: { toDate: () => new Date('2025-09-02') } },
                    { id: 'sub7', employeeName: 'John Smith', email: 'john@example.com', amount: 80.00, description: 'Fitness Class Pack', purchaseDate: { toDate: () => new Date('2025-09-11') } },
                ];
                const MOCK_PENDING_LIST = [
                    { id: 'sub5', employeeName: 'Jane Doe', email: 'employee@example.com', amount: 250.00, description: 'Rock Climbing Gear', purchaseDate: { toDate: () => new Date('2025-09-20') }, receiptUrl: 'https://placehold.co/600x800?text=Receipt+1' },
                    { id: 'sub8', employeeName: 'Peter Jones', email: 'peter@example.com', amount: 45.00, description: 'Wellness App Subscription', purchaseDate: { toDate: () => new Date('2025-09-18') }, receiptUrl: 'https://placehold.co/600x800?text=Receipt+2' },
                ];
                setPayoutItems(MOCK_ADMIN_PAYOUT_LIST);
                setPendingItems(MOCK_PENDING_LIST);
                setLoading(false);

            }, []);

            const handleUpdateStatus = async (submissionId, newStatus, reason = '') => {
                console.log(`Updating submission ${submissionId} to ${newStatus} for reason: ${reason}`);
                // Mock update
                setPendingItems(prev => prev.filter(item => item.id !== submissionId));
            };

            const generateCSV = () => {
                const headers = ["Date of Purchase", "Employee Name", "Email", "Amount", "Description"];
                const rows = payoutItems.map(item => [
                    item.purchaseDate ? item.purchaseDate.toDate().toLocaleDateString() : 'N/A',
                    item.employeeName,
                    item.email,
                    item.amount.toFixed(2),
                    `"${item.description}"`
                ]);
                const csvContent = "data:text/csv;charset=utf-8," + [headers.join(","), ...rows.map(e => e.join(","))].join("\n");
                const encodedUri = encodeURI(csvContent);
                const link = document.createElement("a");
                link.setAttribute("href", encodedUri);
                link.setAttribute("download", `payroll_report_${new Date().toISOString().split('T')[0]}.csv`);
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            };

            const markAllPaid = async () => {
                setGenerating(true);
                console.log('Marking all as paid...');
                await new Promise(resolve => setTimeout(resolve, 1500));
                setPayoutItems([]); // Clear list for demo
                setGenerating(false);
            };

            if (loading) return <div>Loading Payroll Data...</div>;

            return (
                <div className="space-y-8 w-full max-w-5xl">
                    <EmployeeLookup />
                    <EmployeeUploader />
                    <PendingReview pendingItems={pendingItems} onUpdateStatus={handleUpdateStatus} />

                    <div className="bg-white p-8 rounded-2xl shadow-lg w-full">
                        <h2 className="text-2xl font-bold text-gray-800 mb-2">Payroll Actions</h2>
                        <p className="text-gray-600 mb-6">Review items ready for payout and generate reports.</p>
                        
                        <div className="flex space-x-4 mb-6">
                            <button onClick={generateCSV} disabled={payoutItems.length === 0} className="bg-[#FEC58F] text-[#1E3743] font-bold py-2 px-4 rounded-lg hover:bg-[#fab56e] disabled:bg-gray-400 disabled:text-white">
                                Generate Monthly Payroll Report (.csv)
                            </button>
                            <button onClick={markAllPaid} disabled={payoutItems.length === 0 || generating} className="bg-[#FEC58F] text-[#1E3743] font-bold py-2 px-4 rounded-lg hover:bg-[#fab56e] disabled:bg-gray-400 disabled:text-white flex items-center justify-center">
                                {generating ? <Spinner color="[#1E3743]" /> : 'Mark All as Paid'}
                            </button>
                        </div>
                        
                        <div className="overflow-x-auto">
                            <table className="w-full text-sm text-left text-gray-500">
                                <caption className="p-5 text-lg font-semibold text-left text-gray-900 bg-white">
                                    Submissions Awaiting Payout
                                    <p className="mt-1 text-sm font-normal text-gray-500">
                                        A total of {payoutItems.length} submission(s) are ready for reimbursement.
                                    </p>
                                </caption>
                                <thead className="text-xs text-gray-700 uppercase bg-gray-50">
                                    <tr>
                                        <th className="px-6 py-3">Employee Name</th>
                                        <th className="px-6 py-3">Email</th>
                                        <th className="px-6 py-3">Description</th>
                                        <th className="px-6 py-3">Amount</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    {payoutItems.length > 0 ? payoutItems.map(item => (
                                        <tr key={item.id} className="bg-white border-b hover:bg-gray-50">
                                            <td className="px-6 py-4 font-medium text-gray-900">{item.employeeName}</td>
                                            <td className="px-6 py-4">{item.email}</td>
                                            <td className="px-6 py-4">{item.description}</td>
                                            <td className="px-6 py-4 font-bold text-gray-800">${item.amount.toFixed(2)}</td>
                                        </tr>
                                    )) : (
                                        <tr>
                                            <td colSpan="4" className="text-center py-10 text-gray-500">No submissions are awaiting payout.</td>
                                        </tr>
                                    )}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            );
        };

        const Profile = ({ user, setUserData, userData }) => {
            const [isEditing, setIsEditing] = useState(false);
            const [name, setName] = useState(user.displayName || '');
            const [isLoading, setIsLoading] = useState(false);
            const [alert, setAlert] = useState(null);

            const handleSave = async () => {
                setIsLoading(true);
                setAlert(null);
                
                console.log('Saving new name:', name);
                await new Promise(res => setTimeout(res, 1000));
                setUserData(prev => ({ ...prev, name }));
                setIsEditing(false);
                setIsLoading(false);
            };

            return (
                <div className="bg-white p-8 rounded-2xl shadow-lg w-full max-w-md">
                    <h2 className="text-2xl font-bold text-gray-800 mb-6">My Profile</h2>
                    {alert && <Alert message={alert.message} type={alert.type} onClose={() => setAlert(null)} />}
                    <div className="space-y-4">
                        <div>
                            <label className="block text-sm font-medium text-gray-700">Full Name</label>
                            <input type="text" value={name} onChange={(e) => setName(e.target.value)} disabled={!isEditing} className="mt-1 block w-full bg-gray-100 disabled:bg-gray-100 enabled:bg-white border-gray-300 rounded-md shadow-sm p-2" />
                        </div>
                        <div>
                            <label className="block text-sm font-medium text-gray-700">Email</label>
                            <input type="email" value={user.email || 'N/A'} disabled className="mt-1 block w-full bg-gray-100 border-gray-300 rounded-md shadow-sm p-2" />
                        </div>
                        <div>
                            <label className="block text-sm font-medium text-gray-700">Associate ID</label>
                            <input type="text" value={userData.associateId || 'N/A'} disabled className="mt-1 block w-full bg-gray-100 border-gray-300 rounded-md shadow-sm p-2" />
                        </div>

                        <div className="flex justify-end space-x-4 pt-4">
                            {isEditing ? (
                                <button onClick={handleSave} disabled={isLoading} className="bg-[#1E3743] text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-700 flex items-center justify-center">
                                    {isLoading ? <Spinner /> : 'Save Changes'}
                                </button>
                            ) : (
                                <button onClick={() => setIsEditing(true)} className="bg-[#FEC58F] text-[#1E3743] font-bold py-2 px-4 rounded-lg hover:bg-[#fab56e]">
                                    Edit Profile
                                </button>
                            )}
                        </div>
                    </div>
                </div>
            );
        };


        // --- APP CONTAINER & AUTH LOGIC ---
        function App() {
            const [user, setUser] = useState(null);
            const [userData, setUserData] = useState({});
            const [loading, setLoading] = useState(true);
            const [isAdmin, setIsAdmin] = useState(false);
            const [currentView, setCurrentView] = useState('dashboard'); // 'dashboard', 'profile'

            useEffect(() => {
                if (SKIP_LOGIN) {
                    const mockUser = START_AS_ADMIN ? MOCK_ADMIN : MOCK_USER;
                    const mockUserData = {
                        name: mockUser.displayName,
                        email: mockUser.email,
                        associateId: START_AS_ADMIN ? 'ADM-888' : 'EMP-12345'
                    }
                    setUser(mockUser);
                    setUserData(mockUserData);
                    setIsAdmin(START_AS_ADMIN);
                    setLoading(false);
                    setCurrentView('dashboard');
                    return;
                }
                setLoading(false);
            }, []);

            const handleSignOut = async () => {
                setUser(null);
                setIsAdmin(false);
                setCurrentView('dashboard');
            };

            if (loading) {
                return (
                    <div className="bg-gray-50 min-h-screen flex items-center justify-center" style={{color: '#1E3743'}}>
                        <h1 className="text-2xl font-bold">Loading Portal...</h1>
                    </div>
                );
            }

            const renderContent = () => {
                if (currentView === 'profile') {
                    return <Profile user={user} userData={userData} setUserData={setUserData} />;
                }
                return isAdmin ? <AdminDashboard /> : <EmployeeDashboard user={user} />;
            };
            
            const NavButton = ({ view, label }) => (
                <button onClick={() => setCurrentView(view)} className={`font-bold py-2 px-4 rounded-lg ${currentView === view ? 'bg-[#1E3743] text-white' : 'text-[#1E3743] hover:bg-gray-200'}`}>
                    {label}
                </button>
            );

            return (
                <div className="bg-gray-50 min-h-screen font-sans" style={{color: '#1E3743'}}>
                    <header className="bg-white shadow-sm">
                        <nav className="container mx-auto px-6 py-4 flex justify-between items-center">
                            <div className="text-2xl font-bold">
                                Wellness Portal
                            </div>
                            <div className="flex items-center space-x-4">
                                {user ? (
                                    <>
                                        <span className="text-gray-600">Welcome, {user.displayName}!</span>
                                        <NavButton view="dashboard" label={isAdmin ? 'Admin Dashboard' : 'Dashboard'} />
                                        <NavButton view="profile" label="Profile" />
                                        <button onClick={handleSignOut} className="bg-[#1E3743] text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-700">Sign Out</button>
                                    </>
                                ) : (
                                    <p>Please sign in.</p>
                                )}
                            </div>
                        </nav>
                    </header>

                    <main className="container mx-auto px-6 py-12 flex flex-col items-center">
                        {!user ? (
                            <AuthScreen setUser={setUser} />
                        ) : (
                            renderContent()
                        )}
                    </main>
                </div>
            );

        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
