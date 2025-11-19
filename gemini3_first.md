Here are the code blocks for the requested modifications.

### 1. Update Frontend Types (`src/types.ts`)

I added the `emailAddressBsness` and `mobilePhoneNumberBsness` fields to the `CompanyBusinessPlace` and `BusinessPlace` interfaces.

```typescript
// leads_webapp1/src/types.ts

// A generic type for different registration types
export type RegistrationType = 'company' | 'business-name';

// --- Shared Person/Contact Interfaces ---
export interface PersonDetail {
    dateOfBirth: string;
    firstName: string;
    middleName: string;
    lastName: string;
    nationalId: string;
    emailAddress: string;
    mobilePhoneNumber: string;
    passportNumber: string;
    tin?: string;
    type?: string; // "Natural person" or "Other"
    originOf: string;
    nationality: string;
    verification?: string;
    country: string;
    genderFemale: boolean;
    genderMale: boolean;
    typeOfLocalAddress?: string;
    region: string;
    district: string;
    ward: string;
    postcode: string;
    addressUnSurveyed?: string;
    address?: string;
    street: string;
    road: string;
    plotNumber: string;
    blockNumber: string;
    houseNumber: string;
    shareholderId?: string;
}

export interface Applicant {
    trackingNumber: string;
    timeApplicationCreated: string;
    timeApplicationSubmitted: string;
    approvalStatus: string;
    serviceType: string;
    dateOfBirth: string;
    firstName: string;
    middleName: string;
    lastName: string;
    emailAddress: string;
    passportNumber: string;
    nationalityApplicant: string;
    mobilePhoneNumber: string;
    nationalID: string;
    genderMale: boolean;
    genderFemale: boolean;
}

// --- Company Specific Interfaces ---
export interface CompanyInfo {
    companyType: string;
    companyName: string;
    incorporationDate: string;
    accountingDate: string;
    incorporationNumber: string;
    tin: string;
    authorisedShare?: string;
}

export interface CompanyBusinessPlace {
    country: string;
    typeOfLocalAddress: string;
    region: string;
    district: string;
    ward: string;
    postcode: string;
    poBox: string;
    emailAddressCmpny: string;
    mobilePhoneNumberCmpny: string;
    streetBusiness: string;
    roadBusiness: string;
    plotNumberBusiness: string;
    blockNumberBusiness: string;
    houseNumberBusiness: string;
    // Added per request
    emailAddressBsness?: string;
    mobilePhoneNumberBsness?: string;
}

export interface CompanySecretary {
    // ... same structure as PersonDetail
    [key: string]: any;
}

export interface OtherSections {
    authorisedShare: string;
    numberOfShares: string;
    sharesTaken: string;
    numberOfRecordsPagination: string;
    paymentOrderDate: string;
}

export interface TimelineEntry {
    recordId: string;
    serviceType: string;
    timeApplicationCreated: string;
    timeApplicationSubmitted: string;
}

export interface Company {
    id: string; // This is the RECORD id
    registrationType: 'company';
    applicantObj: Applicant;
    companyInfoObj: CompanyInfo;
    principalPlaceOfBusinessObj: CompanyBusinessPlace;
    allBusinessActivitiesArray: string[];
    directorDetail: PersonDetail[];
    shareholderDetail: PersonDetail[]; // Can contain persons or corporate shells
    companySecretaryDetail: CompanySecretary;
    otherRemainingSections: OtherSections;
    timeline: TimelineEntry[];
    highlight?: { [key: string]: string[] };
}

// --- Business Name Specific Interfaces ---
export interface BusinessInfo {
    businessType: string;
    businessName: string;
    registrationDate: string;
    registrationNumber: string;
    state: string;
}

export interface BusinessPlace {
    typeOfLocalAddress: string;
    unsurveyedAddress: string;
    region: string;
    district: string;
    ward: string;
    postcode: string;
    poBox: string;
    emailAddressBsness: string;
    mobilePhoneNumberBsness: string;
    streetBusiness: string;
    roadBusiness: string;
    plotNumberBusiness: string;
    blockNumberBusiness: string;
    houseNumberBusiness: string;
}

export interface BusinessName {
    id: string;
    registrationType: 'business-name';
    applicantObj: Applicant;
    businessInfoObj: BusinessInfo;
    principalPlaceOfBusinessObj: BusinessPlace;
    allBusinessActivitiesArray: string[];
    ownerDetail: PersonDetail[];
    personsWhoDetail: PersonDetail[];
    otherBankDetail: PersonDetail[];
    highlight?: { [key: string]: string[] };
}

// --- NEW Corporate Shareholder Types ---
export interface CorporateShareholderListItem {
    id: string;
    name: string;
    country: string;
    shareholdingCount: number;
    highlight?: { [key: string]: string[] };
}

export interface Shareholding {
    companyId: string; // This is the company's RECORD ID for linking to the detail page
    companyName: string;
    sharesClass: string;
    sharesCount: number;
    approvalStatus?: string;
}

export interface CorporateShareholderDetail {
    id: string;
    name: string;
    country: string;
    emailAddress: string;
    phoneNumber: string;
    shareholdings: Shareholding[];
}

// --- Union and Generic Types ---
export type Registration = Company | BusinessName;

// Generic list item for dashboards
export interface RegistrationListItem {
    id: string; // This is the RECORD id
    registrationType: RegistrationType;
    applicantObj: {
        approvalStatus: string;
        timeApplicationCreated: string;
    };
    companyInfoObj?: CompanyInfo;
    businessInfoObj?: BusinessInfo;
    highlight?: { [key: string]: string[] };
}

// --- People and Contact Search Types ---
export interface Person {
    id: string;
    fullName: string;
    dateOfBirth: string | null;
    nationality: string;
    role: string;
    registrationId: string; // This is incorporationNumber for companies (for grouping)
    sourceRecordId: string; // The specific record ID for linking
    registrationName: string;
    registrationType: RegistrationType;
    highlight?: { [key: string]: string[] };
}

export interface PersonRole {
    role: string; // This can now be comma-separated roles
    registrationId: string; // The stable identifier (e.g. incorporationNumber)
    sourceRecordId: string; // The specific record ID for linking
    registrationName: string;
    registrationType: RegistrationType;
    approvalStatus?: string;
}

export interface PersonDetails {
    fullName: string;
    dateOfBirth: string | null;
    nationality: string;
}

export type PersonProfileDetails = Partial<
    Omit<PersonDetail, 'emailAddress' | 'mobilePhoneNumber'> &
    Omit<Applicant, 'emailAddress' | 'mobilePhoneNumber'> & {
        nida_region?: string;
        nida_district?: string;
        nida_ward?: string;
        emailAddress: string[];
        mobilePhoneNumber: string[];
    }
>;

export interface ShareholdingInfo {
    companyId: string; // This is the specific record ID
    companyName: string;
    shareValue: number;
}

export interface AssociatedTin {
    id: string;
    tinNo: string;
    name: string | null;
    tinType: string | null;
    postalCity: string | null;
    businessStartDate: string | null;
    telephones: string[];
}

export interface FullPersonProfile {
    details: PersonDetails;
    roles: PersonRole[];
    detailedInfo: PersonProfileDetails | null;
    shareholdings?: ShareholdingInfo[];
    associatedTins?: AssociatedTin[];
}

export interface CompanyContactResult {
    companyId: string; // This is the record ID
    companyName: string;
    foundInSections: string;
    registrationType: 'company';
    approvalStatus?: string;
}

export interface BusinessNameContactResult {
    businessNameId: string;
    businessName: string;
    foundInSections: string;
    registrationType: 'business-name';
    approvalStatus?: string;
}

export interface PersonContactResult {
    personId: string;
    fullName: string;
    role: string;
    registrationId: string;
    registrationName: string;
    registrationType: RegistrationType;
    approvalStatus?: string;
}

export interface ContactSearchData {
    companies: CompanyContactResult[];
    business_names: BusinessNameContactResult[];
    people: PersonContactResult[];
    associatedTins: AssociatedTin[];
}

// --- Network Graph Types ---
export interface GraphNode {
    id: string;
    name: string;
    type: 'person' | 'company' | 'business-name' | 'corporate-shareholder';
    val?: number;
    mostCommonRole?: string;
    dateOfBirth?: string;
    gender?: 'male' | 'female';
}

export interface GraphLink {
    source: string;
    target: string;
    roles: string[];
}

export interface GraphData {
    nodes: GraphNode[];
    links: GraphLink[];
}

// --- Key Influencers Types ---
export interface LeaderboardPerson {
    id: string;
    name: string;
    value: number;
    nationality: string | null;
}

export interface CoDirectorMatrixEntry {
    person1_id: string;
    person1_name: string;
    person2_id: string;
    person2_name: string;
    count: number;
}

export interface CoDirectorMatrixData {
    matrix: CoDirectorMatrixEntry[];
    people: { id: string, name: string }[];
}

export interface PathNode {
    id: string;
    name: string;
    type: string;
}

export interface PathLink {
    source: string;
    target: string;
    label: string;
}

export interface SixDegreesPath {
    path_found: boolean;
    message: string;
    nodes?: PathNode[];
    links?: PathLink[];
}

export interface PhoneFilterPerson {
    id: string;
    fullName: string;
    age: number | null;
    mobilePhoneNumber: string | null;
    nationality: string | null;
    formattedNumber: string | null;
    standardNumber: string | null;
    highlight?: { [key: string]: string[] };
    isEntity?: boolean; // New field to identify if it's a company/BN
}

// --- NEW: Contact Exporter Types ---
export interface ContactExportPreviewStats {
    initial_count: number;
    duplicates_removed: number;
    engaged_removed: number;
    final_count: number;
    sample_size: number;
}

export interface ContactExportPreviewData {
    stats: ContactExportPreviewStats;
    sample_data_head: { Name: string; 'Phone Number': string }[];
    sample_data_tail: { Name: string; 'Phone Number': string }[];
}

// --- TRA TINs Types ---
export interface TraTinListItem {
    id: string;
    tinNo: string;
    name: string | null;
    tinType: string | null;
    postalCity: string | null;
    businessStartDate: string | null;
    telephones: string[];
    age: number | null;
    highlight?: { [key: string]: string[] };
}

export interface TraTinDetail extends TraTinListItem {
    firstname?: string;
    middlename?: string;
    lastname?: string;
    national_id?: string;
    pobox?: string;
    postal_city?: string;
    date_of_birth?: string;
    nida_region?: string;
    nida_district?: string;
    nida_ward?: string;
}

// --- Wabunge Types ---
export interface WabungeListItem {
    id: string;
    name: string;
    title: string | null;
    phone1: string | null;
    phone2: string | null;
    chama: string | null;
    address1: string | null;
    address2: string | null;
    highlight?: { [key: string]: string[] };
}

export interface WabungeDetail extends WabungeListItem {}

// --- Place of Business Types ---
export interface Place {
    id: string; // base64 encoded key
    placeName: string;
    placeType: string;
    count: number;
}
```

### 2. Update Company Detail Page (`src/pages/details/CompanyDetailPage.tsx`)

Added the `mobilePhoneNumberBsness` and `emailAddressBsness` display logic.

```tsx
// leads_webapp1/src/pages/details/CompanyDetailPage.tsx
import React, { useState, useEffect, useMemo } from 'react';
import { useParams, Link } from 'react-router-dom';
import { DetailCard } from '../../components/DetailCard';
import { PersonDetail, Company, TimelineEntry } from '../../types';
import { HtmlContent } from '../../components/HtmlContent';
import { getFlagImagePathForNationality } from '../../utils/nationalityUtils';

const API_URL = import.meta.env.VITE_API_BASE_URL;

const renderValue = (value: string | boolean | number | undefined | null) => {
    if (typeof value === 'boolean') {
        return value ? 'Yes' : 'No';
    }
    if (typeof value === 'number' && !isNaN(value)) {
        return value.toLocaleString('en-US');
    }
    return value && value !== 'noResult' && String(value).trim() !== '' ? String(value) : <span className="text-slate-500 dark:text-slate-400">N/A</span>;
};

const renderContactLink = (value: string | undefined | null) => {
    const cleanValue = value && value !== 'noResult' && String(value).trim() !== '' ? String(value) : null;
    if (!cleanValue) {
        return <span className="text-slate-500 dark:text-slate-400">N/A</span>;
    }
    return (
        <Link to={`/contacts?q=${encodeURIComponent(cleanValue)}`} className="text-primary hover:underline">
            {cleanValue}
        </Link>
    );
};

const BackIcon: React.FC = () => (
    <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor"><path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" /></svg>
);

const calculateAge = (dobString: string | null | undefined): number | string => {
    if (!dobString || dobString === 'noResult') return 'N/A';
    const parts = dobString.split('/');
    if (parts.length !== 3) return 'N/A';
    const birthDate = new Date(`${parts[2]}-${parts[1]}-${parts[0]}`);
    if (isNaN(birthDate.getTime())) return 'N/A';
    const ageDiffMs = Date.now() - birthDate.getTime();
    const ageDate = new Date(ageDiffMs);
    return Math.abs(ageDate.getUTCFullYear() - 1970);
};

const getGender = (person: { genderMale?: boolean, genderFemale?: boolean } | null | undefined): React.ReactNode => {
    if (!person) return renderValue(null);
    if (person.genderMale) {
        return (
            <span className="inline-flex items-center gap-2">
                <span className="material-icons text-blue-400" style={{ fontSize: '1.2rem' }}>male</span> Male
            </span>
        );
    }
    if (person.genderFemale) {
        return (
            <span className="inline-flex items-center gap-2">
                <span className="material-icons text-pink-400" style={{ fontSize: '1.2rem' }}>female</span> Female
            </span>
        );
    }
    return renderValue(null);
};

const Timeline: React.FC<{ entries: TimelineEntry[], currentRecordId: string }> = ({ entries, currentRecordId }) => {
    if (!entries || entries.length === 0) return null;

    return (
        <DetailCard title="Company History & Filings">
            <ol className="relative border-l border-slate-200 dark:border-slate-700 ml-4">
                {entries.map((entry) => {
                    const isCurrent = entry.recordId === currentRecordId;
                    return (
                        <li key={entry.recordId} className="mb-6 ml-6">
                            <span className={`absolute flex items-center justify-center w-6 h-6 rounded-full -left-3 ring-8 ring-white dark:ring-slate-900 ${isCurrent ? 'bg-primary' : 'bg-blue-100 dark:bg-blue-900'}`}>
                                <span className={`material-icons text-base ${isCurrent ? 'text-white' : 'text-blue-800 dark:text-blue-300'}`}>
                                    {isCurrent ? 'history_toggle_off' : 'history'}
                                </span>
                            </span>
                            <Link to={`/company/detail/${entry.recordId}`} className="hover:underline">
                                <h3 className={`flex items-center mb-1 text-lg font-semibold ${isCurrent ? 'text-primary' : 'text-slate-900 dark:text-white'}`}>
                                    {entry.serviceType || 'Update'}
                                    {isCurrent && <span className="bg-blue-100 text-primary text-sm font-medium mr-2 px-2.5 py-0.5 rounded dark:bg-blue-900 dark:text-blue-300 ml-3">Current</span>}
                                </h3>
                            </Link>
                            <time className="block mb-2 text-sm font-normal leading-none text-slate-400 dark:text-slate-500">
                                Created: {entry.timeApplicationCreated || 'N/A'}
                            </time>
                            <p className="text-sm text-slate-500 dark:text-slate-400">Record ID: {entry.recordId}</p>
                        </li>
                    );
                })}
            </ol>
        </DetailCard>
    );
};

const CompanyDetailPage: React.FC = () => {
    const { id } = useParams<{ id: string }>(); // id is now the record ID
    const [company, setCompany] = useState<Company | null>(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        if (!id) return;
        const fetchCompanyDetails = async () => {
            try {
                setLoading(true);
                setError(null);
                const response = await fetch(`${API_URL}/api/companies/${id}`);
                if (!response.ok) {
                    const errData = await response.json();
                    throw new Error(errData.detail || `HTTP error! status: ${response.status}`);
                }
                const data = await response.json();
                if (!data.success) throw new Error(data.message || 'Failed to fetch company details.');
                setCompany(data.company);
            } catch (e: any) {
                setError(e.message);
                console.error("Failed to fetch company details:", e);
            } finally {
                setLoading(false);
            }
        };
        fetchCompanyDetails();
    }, [id]);

    const shareData = useMemo(() => {
        if (!company?.otherRemainingSections) {
            return { valuePerShare: 0, shareholderShares: new Map<string, number>() };
        }
        const { authorisedShare, numberOfShares, sharesTaken } = company.otherRemainingSections;
        const authShareNum = parseFloat(authorisedShare?.replace(/,/g, ''));
        const numSharesNum = parseFloat(numberOfShares?.replace(/,/g, ''));
        const valuePerShare = (authShareNum > 0 && numSharesNum > 0) ? authShareNum / numSharesNum : 0;

        const shareholderShares = new Map<string, number>();
        if (sharesTaken) {
            const tempDiv = document.createElement('div');
            tempDiv.innerHTML = sharesTaken;
            const rows = tempDiv.querySelectorAll('tbody tr');
            rows.forEach(row => {
                const cells = row.querySelectorAll('td');
                if (cells.length >= 4) {
                    const shareholderId = cells[0]?.textContent?.trim() || '';
                    const sharesStr = cells[3]?.textContent?.trim().replace(/<.*?>/g, '').replace(/,/g, '') || '0';
                    const shares = parseInt(sharesStr, 10);
                    if (shareholderId && !isNaN(shares)) {
                        shareholderShares.set(shareholderId, shares);
                    }
                }
            });
        }
        return { valuePerShare, shareholderShares };
    }, [company]);

    const totalShares = useMemo(() => {
        if (!shareData.shareholderShares || shareData.shareholderShares.size === 0) {
            return 0;
        }
        let sum = 0;
        for (const count of shareData.shareholderShares.values()) {
            sum += count;
        }
        return sum;
    }, [shareData]);

    if (loading) return <div className="text-center text-slate-900 dark:text-white text-2xl">Loading Company Details...</div>;
    if (error) return <div className="text-center text-red-500 text-lg p-4 bg-red-500/10 rounded-md">Error: {error}</div>;
    if (!company) return <div className="text-center text-slate-500 dark:text-slate-400 text-2xl">Company not found.</div>;

    const { applicantObj, companyInfoObj, principalPlaceOfBusinessObj, allBusinessActivitiesArray, directorDetail, shareholderDetail, companySecretaryDetail, otherRemainingSections, timeline } = company;

    const renderPartyDetails = (title: string, details: PersonDetail[]) => {
        const isShareholder = title === "Shareholders";
        return (
            <DetailCard title={title}>
                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                    {details.map((detail, index) => {
                        const isCorporate = detail.type === 'Other';
                        const name = isCorporate ? (detail.lastName || detail.firstName || 'Corporate Entity') : `${detail.firstName || ''} ${detail.middleName || ''} ${detail.lastName || ''}`.replace(/\s+/g, ' ').trim();
                        const identifier = isCorporate ? detail.shareholderId : detail.nationalId || detail.passportNumber;
                        const link = isCorporate ? `/corporate-shareholder/${identifier}` : `/person/${identifier}`;
                        const flagPath = getFlagImagePathForNationality(detail.nationality);

                        const sharesOwned = detail.shareholderId ? (shareData.shareholderShares.get(detail.shareholderId) || 0) : 0;
                        const totalValue = sharesOwned * shareData.valuePerShare;
                        const percentage = totalShares > 0 ? (sharesOwned / totalShares) * 100 : 0;

                        return (
                            <div key={`${identifier}-${index}`} className="bg-slate-100 dark:bg-slate-800 p-4 rounded-lg border border-slate-200 dark:border-slate-700">
                                <h4 className="font-bold text-lg text-slate-900 dark:text-white mb-2 truncate">
                                    {identifier ? <Link to={link} className="text-primary hover:underline">{name}</Link> : name}
                                </h4>
                                <div className="space-y-1 text-sm">
                                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Type:</span> {renderValue(detail.type)}</p>
                                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Email:</span> {renderContactLink(detail.emailAddress)}</p>
                                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Phone:</span> {renderContactLink(detail.mobilePhoneNumber)}</p>
                                    <p className="inline-flex items-center">
                                        <span className="font-semibold text-slate-500 dark:text-slate-400">{isCorporate ? 'Country' : 'Nationality'}:</span>
                                        {!isCorporate && flagPath && <img src={flagPath} alt="flag" className="inline-block h-4 w-5 ml-2 mr-1" />}
                                        <span className="ml-1">{renderValue(isCorporate ? detail.country : detail.nationality)}</span>
                                    </p>

                                    {!isCorporate && (
                                        <>
                                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Date of Birth:</span> {renderValue(detail.dateOfBirth)}</p>
                                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Approx. Age:</span> {calculateAge(detail.dateOfBirth)}</p>
                                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Gender:</span> {getGender(detail)}</p>
                                        </>
                                    )}

                                    {isShareholder && sharesOwned > 0 && (
                                        <div className="pt-2 mt-2 border-t border-slate-200 dark:border-slate-700">
                                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Shares Held:</span> {sharesOwned.toLocaleString()}</p>
                                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Est. Value:</span>{' '}
                                                {totalValue.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })} TSh
                                            </p>
                                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Share Percentage:</span> {percentage.toFixed(2)}%</p>
                                        </div>
                                    )}
                                </div>
                            </div>
                        );
                    })}
                </div>
            </DetailCard>
        );
    };

    const status = applicantObj.approvalStatus && applicantObj.approvalStatus !== 'noResult' ? applicantObj.approvalStatus : 'Unknown';
    const applicantFullName = `${applicantObj.firstName || ''} ${applicantObj.middleName || ''} ${applicantObj.lastName || ''}`.replace(/\s+/g, ' ').trim();
    const applicantIdentifier = applicantObj.nationalID || applicantObj.passportNumber;
    const applicantFlagPath = getFlagImagePathForNationality(applicantObj.nationalityApplicant);

    const secretaryFullName = companySecretaryDetail ? `${companySecretaryDetail.firstName || ''} ${companySecretaryDetail.middleName || ''} ${companySecretaryDetail.lastName || ''}`.replace(/\s+/g, ' ').trim() : '';
    const secretaryIdentifier = companySecretaryDetail ? companySecretaryDetail.nationalId || companySecretaryDetail.passportNumber : null;
    const secretaryFlagPath = companySecretaryDetail ? getFlagImagePathForNationality(companySecretaryDetail.nationality) : null;

    return (
        <div className="space-y-6">
            <Link to={-1 as any} className="inline-flex items-center gap-2 text-primary hover:underline mb-4">
                <BackIcon /> Back
            </Link>

            <div className="bg-white dark:bg-slate-900 p-6 rounded-lg shadow-sm border border-slate-200 dark:border-slate-800">
                <div className="flex justify-between items-start flex-wrap gap-4">
                    <div>
                        <h1 className="text-3xl lg:text-4xl font-extrabold text-slate-900 dark:text-white">{companyInfoObj.companyName || 'Unnamed Company'}</h1>
                        <p className="text-slate-500 dark:text-slate-400 mt-1">
                            Incorporation No: {companyInfoObj.incorporationNumber} | Record ID: {company.id}
                        </p>
                    </div>
                    <div>
                        <span className={`px-4 py-2 text-sm font-bold rounded-full whitespace-nowrap ${
                            status === 'Approve' ? 'bg-emerald-100 text-emerald-800 dark:bg-emerald-900/50 dark:text-emerald-300' : 'bg-amber-100 text-amber-800 dark:bg-amber-900/50 dark:text-amber-300'
                        }`}>
                            {status}
                        </span>
                    </div>
                </div>
            </div>

            <Timeline entries={timeline} currentRecordId={company.id} />

            <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <DetailCard title="Company Information">
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Type:</span> {renderValue(companyInfoObj.companyType)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">TIN:</span> {renderValue(companyInfoObj.tin)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Incorporation Date:</span> {renderValue(companyInfoObj.incorporationDate)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Accounting Date:</span> {renderValue(companyInfoObj.accountingDate)}</p>
                </DetailCard>
                <DetailCard title="Share Capital">
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Authorised Share Capital:</span> {renderValue(parseFloat(otherRemainingSections.authorisedShare).toLocaleString())} TSh</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Total Number of Shares:</span> {renderValue(parseFloat(otherRemainingSections.numberOfShares).toLocaleString())}</p>
                    <div className='mt-2 pt-2 border-t border-slate-200 dark:border-slate-700'>
                        <p className='font-bold text-slate-800 dark:text-white'><span className="font-semibold text-slate-500 dark:text-slate-400">Value per Share:</span> {renderValue(shareData.valuePerShare)} TSh</p>
                    </div>
                </DetailCard>
            </div>

            <DetailCard title="Applicant Information">
                <div className="grid grid-cols-1 md:grid-cols-2 gap-x-6 gap-y-2">
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Name:</span>{' '}
                        {applicantIdentifier ? (
                            <Link to={`/person/${applicantIdentifier}`} className="text-primary hover:underline">{renderValue(applicantFullName)}</Link>
                        ) : (renderValue(applicantFullName))}
                    </p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">National ID:</span> {renderValue(applicantObj.nationalID)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Email:</span> {renderContactLink(applicantObj.emailAddress)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Phone:</span> {renderContactLink(applicantObj.mobilePhoneNumber)}</p>
                    <p className="inline-flex items-center"><span className="font-semibold text-slate-500 dark:text-slate-400">Nationality:</span> {applicantFlagPath && <img src={applicantFlagPath} alt="flag" className="inline-block h-4 w-5 ml-2 mr-1" />} <span className="ml-1">{renderValue(applicantObj.nationalityApplicant)}</span> </p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Date of Birth:</span> {renderValue(applicantObj.dateOfBirth)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Approx. Age:</span> {calculateAge(applicantObj.dateOfBirth)}</p>
                    <p><span className="font-semibold text-slate-500 dark:text-slate-400">Gender:</span> {getGender(applicantObj)}</p>
                </div>
            </DetailCard>

            {companySecretaryDetail && (companySecretaryDetail.nationalId || companySecretaryDetail.passportNumber) && (
                <DetailCard title="Company Secretary">
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-x-6 gap-y-2">
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">Name:</span>{' '}
                            {secretaryIdentifier ? (
                                <Link to={`/person/${secretaryIdentifier}`} className="text-primary hover:underline">{renderValue(secretaryFullName)}</Link>
                            ) : (renderValue(secretaryFullName))}
                        </p>
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">National ID:</span> {renderValue(companySecretaryDetail.nationalId)}</p>
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">Email:</span> {renderContactLink(companySecretaryDetail.emailAddress)}</p>
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">Phone:</span> {renderContactLink(companySecretaryDetail.mobilePhoneNumber)}</p>
                        <p className="inline-flex items-center"><span className="font-semibold text-slate-500 dark:text-slate-400">Nationality:</span> {secretaryFlagPath && <img src={secretaryFlagPath} alt="flag" className="inline-block h-4 w-5 ml-2 mr-1" />} <span className="ml-1">{renderValue(companySecretaryDetail.nationality)}</span> </p>
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">Date of Birth:</span> {renderValue(companySecretaryDetail.dateOfBirth)}</p>
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">Approx. Age:</span> {calculateAge(companySecretaryDetail.dateOfBirth)}</p>
                        <p><span className="font-semibold text-slate-500 dark:text-slate-400">Gender:</span> {getGender(companySecretaryDetail)}</p>
                    </div>
                </DetailCard>
            )}

            {principalPlaceOfBusinessObj && (
                <DetailCard title="Principal Place of Business">
                    <div className="grid grid-cols-2 md:grid-cols-3 gap-4">
                        <div><span className="font-semibold text-slate-500 dark:text-slate-400">Region:</span><p>{renderValue(principalPlaceOfBusinessObj.region)}</p></div>
                        <div><span className="font-semibold text-slate-500 dark:text-slate-400">District:</span><p>{renderValue(principalPlaceOfBusinessObj.district)}</p></div>
                        <div><span className="font-semibold text-slate-500 dark:text-slate-400">Ward:</span><p>{renderValue(principalPlaceOfBusinessObj.ward)}</p></div>
                        <div className="col-span-full">
                            <span className="font-semibold text-slate-500 dark:text-slate-400">Official Email:</span>
                            <p>{renderContactLink(principalPlaceOfBusinessObj.emailAddressBsness || principalPlaceOfBusinessObj.emailAddressCmpny)}</p>
                        </div>
                        <div className="col-span-full">
                            <span className="font-semibold text-slate-500 dark:text-slate-400">Official Phone:</span>
                            <p>{renderContactLink(principalPlaceOfBusinessObj.mobilePhoneNumberBsness || principalPlaceOfBusinessObj.mobilePhoneNumberCmpny)}</p>
                        </div>
                        <div className="col-span-full"><span className="font-semibold text-slate-500 dark:text-slate-400">Address:</span><p>{renderValue([principalPlaceOfBusinessObj.plotNumberBusiness, principalPlaceOfBusinessObj.streetBusiness, principalPlaceOfBusinessObj.blockNumberBusiness, principalPlaceOfBusinessObj.roadBusiness].filter(Boolean).join(', '))}</p></div>
                    </div>
                </DetailCard>
            )}

            <DetailCard title="Business Activities">
                <ul className="list-none space-y-2 text-slate-800 dark:text-slate-300">
                    {allBusinessActivitiesArray && allBusinessActivitiesArray.length > 0 ? allBusinessActivitiesArray.map((activity, index) => {
                        return (
                            <li key={index}>
                                <Link to={`/company/activity/${encodeURIComponent(activity)}`} className="text-primary hover:underline">
                                    {activity.replace(/#\d+\s/, '')}
                                </Link>
                            </li>
                        );
                    }) : <p className="text-slate-500 dark:text-slate-400">No activities listed.</p>}
                </ul>
            </DetailCard>

            {directorDetail && directorDetail.length > 0 && renderPartyDetails("Directors", directorDetail)}
            {shareholderDetail && shareholderDetail.length > 0 && renderPartyDetails("Shareholders", shareholderDetail)}

            {otherRemainingSections.sharesTaken && <DetailCard title="Shares Breakdown"><HtmlContent content={otherRemainingSections.sharesTaken} /></DetailCard>}
        </div>
    );
};

export default CompanyDetailPage;
```

### 3. Update Business Name Detail Page (`src/pages/details/BusinessNameDetailPage.tsx`)

Added the `mobilePhoneNumberBsness` and `emailAddressBsness` display logic.

```tsx
// leads_webapp1/src/pages/details/BusinessNameDetailPage.tsx
import React, { useState, useEffect } from 'react';
import { useParams, Link } from 'react-router-dom';
import { DetailCard } from '../../components/DetailCard';
import { PersonDetail, BusinessName, Applicant } from '../../types';
import { getFlagImagePathForNationality } from '../../utils/nationalityUtils';

const API_URL = import.meta.env.VITE_API_BASE_URL;

const renderValue = (value: any) => value && value !== 'noResult' && String(value).trim() !== '' ? String(value) : <span className="text-slate-500 dark:text-slate-400">N/A</span>;

const renderContactLink = (value: string | undefined | null) => {
    const cleanValue = value && value !== 'noResult' && String(value).trim() !== '' ? String(value) : null;
    if (!cleanValue) return <span className="text-slate-500 dark:text-slate-400">N/A</span>;
    return <Link to={`/contacts?q=${encodeURIComponent(cleanValue)}`} className="text-primary hover:underline">{cleanValue}</Link>;
};

const BackIcon: React.FC = () => (
    <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor"><path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" /></svg>
);

const calculateAge = (dobString: string | null | undefined): number | string => {
    if (!dobString || dobString === 'noResult') return 'N/A';
    const parts = dobString.split('/');
    if (parts.length !== 3) return 'N/A';
    const birthDate = new Date(`${parts[2]}-${parts[1]}-${parts[0]}`);
    if (isNaN(birthDate.getTime())) return 'N/A';
    const ageDiffMs = Date.now() - birthDate.getTime();
    const ageDate = new Date(ageDiffMs);
    return Math.abs(ageDate.getUTCFullYear() - 1970);
};

const getGender = (person: { genderMale?: boolean, genderFemale?: boolean } | null | undefined): React.ReactNode => {
    if (!person) return renderValue(null);
    if (person.genderMale) {
        return (
            <span className="inline-flex items-center gap-2">
                <span className="material-icons text-blue-400" style={{ fontSize: '1.2rem' }}>male</span> Male
            </span>
        );
    }
    if (person.genderFemale) {
        return (
            <span className="inline-flex items-center gap-2">
                <span className="material-icons text-pink-400" style={{ fontSize: '1.2rem' }}>female</span> Female
            </span>
        );
    }
    return renderValue(null);
};

const PersonCard = ({ title, people }: { title: string, people: PersonDetail[] }) => (
    <DetailCard title={title}>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            {people.map((person, index) => {
                const personIdentifier = person.nationalId || person.passportNumber || `person-${index}`;
                const fullName = `${person.firstName || ''} ${person.middleName || ''} ${person.lastName || ''}`.trim();
                const flagPath = getFlagImagePathForNationality(person.nationality);

                return (
                    <div key={personIdentifier} className="bg-slate-100 dark:bg-slate-800 p-4 rounded-lg border border-slate-200 dark:border-slate-700">
                        <h4 className="font-bold text-lg text-slate-900 dark:text-white mb-2 truncate">
                            <Link to={`/person/${personIdentifier}`} className="text-primary hover:underline">{fullName}</Link>
                        </h4>
                        <div className="space-y-1 text-sm">
                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Email:</span> {renderContactLink(person.emailAddress)}</p>
                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Phone:</span> {renderContactLink(person.mobilePhoneNumber)}</p>
                            <p className="inline-flex items-center"><span className="font-semibold text-slate-500 dark:text-slate-400">Nationality:</span> {flagPath && <img src={flagPath} alt="flag" className="inline-block h-4 w-5 ml-2 mr-1" />} <span className="ml-1">{renderValue(person.nationality)}</span> </p>
                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Date of Birth:</span> {renderValue(person.dateOfBirth)}</p>
                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Approx. Age:</span> {calculateAge(person.dateOfBirth)}</p>
                            <p><span className="font-semibold text-slate-500 dark:text-slate-400">Gender:</span> {getGender(person)}</p>
                        </div>
                    </div>
                );
            })}
        </div>
    </DetailCard>
);

const BusinessNameDetailPage: React.FC = () => {
    const { id } = useParams<{ id: string }>();
    const [businessName, setBusinessName] = useState<BusinessName | null>(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        if (!id) return;
        const fetchDetails = async () => {
            setLoading(true);
            setError(null);
            try {
                const response = await fetch(`${API_URL}/api/business-names/${id}`);
                if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                const data = await response.json();
                if (!data.success) throw new Error(data.message || 'Failed to fetch details.');
                setBusinessName(data.business_name);
            } catch (e: any) {
                setError(e.message);
            } finally {
                setLoading(false);
            }
        };
        fetchDetails();
    }, [id]);

    if (loading) return <div className="text-center text-slate-900 dark:text-white text-2xl">Loading Business Name Details...</div>;
    if (error) return <div className="text-center text-red-500 text-2xl">Error: {error}</div>;
    if (!businessName) return <div className="text-center text-slate-500 dark:text-slate-400 text-2xl">Business Name not found.</div>;

    const { businessInfoObj, principalPlaceOfBusinessObj, allBusinessActivitiesArray, ownerDetail, personsWhoDetail, otherBankDetail } = businessName;
    const status = businessName.applicantObj.approvalStatus && businessName.applicantObj.approvalStatus !== 'noResult' ? businessName.applicantObj.approvalStatus : 'Unknown';

    return (
        <div className="space-y-6">
            <Link to={-1 as any} className="inline-flex items-center gap-2 text-primary hover:underline mb-4">
                <BackIcon /> Back
            </Link>

            <div className="bg-white dark:bg-slate-900 p-6 rounded-lg shadow-sm border border-slate-200 dark:border-slate-800">
                <div className="flex justify-between items-start flex-wrap gap-4">
                    <div>
                        <h1 className="text-3xl lg:text-4xl font-extrabold text-slate-900 dark:text-white">{businessInfoObj.businessName || 'Unnamed Business'}</h1>
                        <p className="text-slate-500 dark:text-slate-400 mt-1">Registration ID: {businessName.id}</p>
                    </div>
                    <div>
                        <span className={`px-4 py-2 text-sm font-bold rounded-full whitespace-nowrap ${
                            status === 'Approve' ? 'bg-green-800 text-green-200' : 'bg-yellow-800 text-yellow-200'
                        }`}>
                            {status}
                        </span>
                    </div>
                </div>
            </div>

            <DetailCard title="Business Information">
                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Type:</span> {renderValue(businessInfoObj.businessType)}</p>
                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Registration No:</span> {renderValue(businessInfoObj.registrationNumber)}</p>
                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Registration Date:</span> {renderValue(businessInfoObj.registrationDate)}</p>
                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Status:</span> {renderValue(businessInfoObj.state)}</p>
            </DetailCard>

            <DetailCard title="Principal Place of Business">
                <div className="grid grid-cols-2 md:grid-cols-3 gap-4">
                    <div><span className="font-semibold text-slate-500 dark:text-slate-400">Region:</span><p>{renderValue(principalPlaceOfBusinessObj.region)}</p></div>
                    <div><span className="font-semibold text-slate-500 dark:text-slate-400">District:</span><p>{renderValue(principalPlaceOfBusinessObj.district)}</p></div>
                    <div><span className="font-semibold text-slate-500 dark:text-slate-400">Ward:</span><p>{renderValue(principalPlaceOfBusinessObj.ward)}</p></div>
                     <div className="col-span-full">
                        <span className="font-semibold text-slate-500 dark:text-slate-400">Official Email:</span>
                        <p>{renderContactLink(principalPlaceOfBusinessObj.emailAddressBsness || principalPlaceOfBusinessObj.emailAddressBsness)}</p>
                    </div>
                    <div className="col-span-full">
                        <span className="font-semibold text-slate-500 dark:text-slate-400">Official Phone:</span>
                        <p>{renderContactLink(principalPlaceOfBusinessObj.mobilePhoneNumberBsness || principalPlaceOfBusinessObj.mobilePhoneNumberBsness)}</p>
                    </div>
                    <div className="col-span-full"><span className="font-semibold text-slate-500 dark:text-slate-400">Address:</span><p>{renderValue(principalPlaceOfBusinessObj.unsurveyedAddress)}</p></div>
                </div>
            </DetailCard>

            <DetailCard title="Business Activities">
                <ul className="list-none space-y-2 text-slate-800 dark:text-slate-300">
                    {allBusinessActivitiesArray?.length > 0 ? allBusinessActivitiesArray.map((activity, index) => (
                        <li key={index}>
                            <Link to={`/business-name/activity/${encodeURIComponent(activity)}`} className="text-primary hover:underline">
                                {activity.replace(/#\d+\s/, '')}
                            </Link>
                        </li>
                    )) : <p className="text-slate-500 dark:text-slate-400">No activities listed.</p>}
                </ul>
            </DetailCard>

            <DetailCard title="Applicant Information">
                <div className="grid grid-cols-1 md:grid-cols-2 gap-x-6 gap-y-2">
                    {businessName.applicantObj ? (() => {
                        const a = businessName.applicantObj;
                        const applicantFullName = `${a.firstName || ''} ${a.middleName || ''} ${a.lastName || ''}`.replace(/\s+/g, ' ').trim();
                        const applicantIdentifier = a.nationalID || a.passportNumber || null;
                        const flagPath = getFlagImagePathForNationality(a.nationalityApplicant);
                        return (
                            <>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Name:</span>{' '}
                                    {applicantIdentifier ? (
                                        <Link to={`/person/${applicantIdentifier}`} className="text-primary hover:underline">{renderValue(applicantFullName)}</Link>
                                    ) : (
                                        renderValue(applicantFullName)
                                    )}
                                </p>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">National ID:</span> {renderValue(a.nationalID)}</p>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Email:</span> {renderContactLink(a.emailAddress)}</p>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Phone:</span> {renderContactLink(a.mobilePhoneNumber)}</p>
                                <p className="inline-flex items-center"><span className="font-semibold text-slate-500 dark:text-slate-400">Nationality:</span> {flagPath && <img src={flagPath} alt="flag" className="inline-block h-4 w-5 ml-2 mr-1" />} <span className="ml-1">{renderValue(a.nationalityApplicant)}</span> </p>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Date of Birth:</span> {renderValue(a.dateOfBirth)}</p>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Approx. Age:</span> {calculateAge(a.dateOfBirth)}</p>
                                <p><span className="font-semibold text-slate-500 dark:text-slate-400">Gender:</span> {getGender(a)}</p>
                            </>
                        );
                    })() : <p className="text-slate-500 dark:text-slate-400">No applicant information available.</p>}
                </div>
            </DetailCard>

            {ownerDetail?.length > 0 && <PersonCard title="Owners" people={ownerDetail} />}
            {personsWhoDetail?.length > 0 && <PersonCard title="Authorized Persons to Update" people={personsWhoDetail} />}
            {otherBankDetail?.length > 0 && <PersonCard title="Other Bank Account Operators" people={otherBankDetail} />}
        </div>
    );
};

export default BusinessNameDetailPage;
```

### 4. Update Phone Filter Page (`src/pages/PhoneFilterPage.tsx`)

Updated the title, description, columns, and included the toggle logic within the Contact Exporter.

```tsx
// leads_webapp1/src/pages/PhoneFilterPage.tsx
import React, { useState, useEffect, useCallback, useMemo, useRef } from 'react';
import { useSearchParams } from 'react-router-dom';
import { PhoneFilterPerson } from '../types';
import { Pagination } from '../components/Pagination';
import { PhoneFilterTable } from '../components/PhoneFilterTable';
import { AdvancedSearchBar, SearchParams } from '../components/AdvancedSearchBar';
import { DetailCard } from '../components/DetailCard';
import { ContactExporter } from '../components/phone_filter/ContactExporter';

const useDebounce = (value: any, delay: number) => {
    const [debouncedValue, setDebouncedValue] = useState(value);
    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);
        return () => {
            clearTimeout(handler);
        };
    }, [value, delay]);
    return debouncedValue;
};

const API_URL = import.meta.env.VITE_API_BASE_URL;
const PEOPLE_PER_PAGE = 10;
type SortKey = 'name' | 'age';
type SortOrder = 'asc' | 'desc';
type UniqueField = 'fullName' | 'formattedNumber' | 'standardNumber' | null;

interface PaginationData {
    currentPage: number;
    totalPages: number;
    totalCount: number;
    searchAfter?: string | null;
}

const PhoneFilterPage: React.FC = () => {
    const [searchParams, setSearchParams] = useSearchParams();
    const searchAfterCache = useRef<Map<number, string>>(new Map());

    // --- State derived from URL ---
    const currentPage = parseInt(searchParams.get('page') || '1', 10);
    const sortBy = (searchParams.get('sortBy') || 'name') as SortKey;
    const sortOrder = (searchParams.get('sortOrder') || 'asc') as SortOrder;
    const uniqueOn = searchParams.get('uniqueOn') as UniqueField;
    const urlSearchParams = useMemo(() => ({
        search: searchParams.get('search') || '',
        wholeWord: searchParams.get('wholeWord') === 'true',
        wholeSentence: searchParams.get('wholeSentence') === 'true',
        nationality: searchParams.get('nationality') || '',
        minAge: searchParams.get('minAge') || '',
        maxAge: searchParams.get('maxAge') || '',
        uniqueOn: searchParams.get('uniqueOn') as UniqueField | null,
    }), [searchParams]);

    // --- Local state for immediate UI feedback on filters ---
    const [nationalityFilter, setNationalityFilter] = useState(urlSearchParams.nationality);
    const [ageFilter, setAgeFilter] = useState({ min: urlSearchParams.minAge, max: urlSearchParams.maxAge });

    const debouncedNationality = useDebounce(nationalityFilter, 500);
    const debouncedAge = useDebounce(ageFilter, 500);

    // --- Component State ---
    const [people, setPeople] = useState<PhoneFilterPerson[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);
    const [paginationData, setPaginationData] = useState<PaginationData>({ currentPage: 1, totalPages: 0, totalCount: 0 });

    useEffect(() => {
        searchAfterCache.current.clear();
    }, [urlSearchParams.search, urlSearchParams.wholeWord, urlSearchParams.wholeSentence, urlSearchParams.nationality, urlSearchParams.minAge, urlSearchParams.maxAge, urlSearchParams.uniqueOn]);

    useEffect(() => {
        setNationalityFilter(urlSearchParams.nationality);
        setAgeFilter({ min: urlSearchParams.minAge, max: urlSearchParams.maxAge });
    }, [urlSearchParams]);

    useEffect(() => {
        const hasChanged = urlSearchParams.nationality !== debouncedNationality || urlSearchParams.minAge !== debouncedAge.min || urlSearchParams.maxAge !== debouncedAge.max;
        if (hasChanged) {
            setSearchParams(prev => {
                debouncedNationality ? prev.set('nationality', debouncedNationality) : prev.delete('nationality');
                debouncedAge.min ? prev.set('minAge', debouncedAge.min) : prev.delete('minAge');
                debouncedAge.max ? prev.set('maxAge', debouncedAge.max) : prev.delete('maxAge');
                if (prev.get('page') !== '1') { prev.set('page', '1'); }
                return prev;
            }, { replace: true });
        }
    }, [debouncedNationality, debouncedAge, setSearchParams, urlSearchParams]);

    useEffect(() => {
        const fetchPeople = async () => {
            setLoading(true);
            setError(null);
            try {
                const params: URLSearchParams = new URLSearchParams({
                    page: String(currentPage),
                    limit: String(PEOPLE_PER_PAGE),
                    sortBy: sortBy,
                    sortOrder: sortOrder,
                });

                if (urlSearchParams.uniqueOn) {
                    let fieldName = '';
                    if (urlSearchParams.uniqueOn === 'fullName') {
                        fieldName = 'full_name.keyword';
                    } else if (['formattedNumber', 'standardNumber'].includes(urlSearchParams.uniqueOn)) {
                        fieldName = 'details.mobilePhoneNumber_normalized';
                    }
                    if (fieldName) {
                        params.append('unique_on_field', fieldName);
                    }
                }

                if (urlSearchParams.search) {
                    params.append('search', urlSearchParams.search);
                    if (urlSearchParams.wholeWord) params.append('wholeWord', 'true');
                    if (urlSearchParams.wholeSentence) params.append('wholeSentence', 'true');
                }
                if (urlSearchParams.nationality) params.append('nationality', urlSearchParams.nationality);
                if (urlSearchParams.minAge) params.append('minAge', urlSearchParams.minAge);
                if (urlSearchParams.maxAge) params.append('maxAge', urlSearchParams.maxAge);

                if (currentPage > 1) {
                    const cursor = searchAfterCache.current.get(currentPage - 1);
                    if (cursor) params.append('searchAfter', cursor);
                }

                const response = await fetch(`${API_URL}/api/phone-filter?${params.toString()}`);
                if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                const data = await response.json();
                if (!data.success) throw new Error(data.message || 'Failed to fetch data.');
                setPeople(data.people);
                setPaginationData(data.pagination);

                if (data.pagination.searchAfter) {
                    searchAfterCache.current.set(currentPage, data.pagination.searchAfter);
                }
            } catch (e: any) {
                setError(e.message);
                console.error("Failed to fetch phone filter data:", e);
            } finally {
                setLoading(false);
            }
        };
        fetchPeople();
    }, [currentPage, urlSearchParams, sortBy, sortOrder]);

    const handleSearchChange = (newSearchParams: SearchParams) => {
        setSearchParams(prev => {
            newSearchParams.term ? prev.set('search', newSearchParams.term) : prev.delete('search');
            newSearchParams.wholeWord ? prev.set('wholeWord', 'true') : prev.delete('wholeWord');
            newSearchParams.wholeSentence ? prev.set('wholeSentence', 'true') : prev.delete('wholeSentence');
            prev.set('page', '1');
            return prev;
        }, { replace: true });
    };

    const handlePageChange = (newPage: number) => {
        if (newPage > 0 && newPage <= paginationData.totalPages && newPage !== currentPage) {
            setSearchParams(prev => { prev.set('page', String(newPage)); return prev; });
            window.scrollTo(0, 0);
        }
    };

    const handleSort = useCallback((column: SortKey) => {
        const newSortOrder = sortBy === column && sortOrder === 'asc' ? 'desc' : 'asc';
        setSearchParams(prev => {
            prev.set('sortBy', column);
            prev.set('sortOrder', newSortOrder);
            prev.set('page', '1');
            return prev;
        }, { replace: true });
    }, [sortBy, sortOrder, setSearchParams]);

    const handleUniqueChange = useCallback((field: UniqueField) => {
        setSearchParams(prev => {
            const currentUniqueOn = prev.get('uniqueOn');
            if (field && currentUniqueOn !== field) {
                prev.set('uniqueOn', field);
            } else {
                prev.delete('uniqueOn');
            }
            prev.set('page', '1');
            return prev;
        }, { replace: true });
    }, [setSearchParams]);

    if (error) {
        return (
            <div className="text-center py-16 px-4 bg-red-500/10 rounded-lg">
                <h2 className="text-2xl font-semibold text-red-700 dark:text-red-400">Failed to Load Data</h2>
                <p className="text-red-600 dark:text-red-500 mt-2">{error}</p>
            </div>
        );
    }

    return (
        <div className="space-y-8">
            <div>
                <h1 className="text-4xl font-bold text-slate-900 dark:text-white mb-2">Phone Number Directory</h1>
                <p className="text-lg text-slate-600 dark:text-slate-400 min-h-[28px]">
                    Browse all individuals and entities with phone numbers associated with companies and business names. {' '}
                    {paginationData.totalCount > 0 ? `Found ${paginationData.totalCount.toLocaleString()} records.` : (loading ? '' : 'Found 0 records.')}
                </p>
            </div>

            <AdvancedSearchBar
                placeholder="Search by name (Person or Entity)..."
                onSearchChange={handleSearchChange}
                initialParams={{ term: urlSearchParams.search, wholeWord: urlSearchParams.wholeWord, wholeSentence: urlSearchParams.wholeSentence }}
            />

            {loading && people.length === 0 ? (
                <div className="text-center py-16 text-slate-800 dark:text-white text-xl">Loading Records...</div>
            ) : (
                <div className={`${loading ? 'opacity-50' : ''} transition-opacity`}>
                    <PhoneFilterTable
                        people={people}
                        nationalityFilter={nationalityFilter}
                        onNationalityChange={setNationalityFilter}
                        ageFilter={ageFilter}
                        onAgeChange={setAgeFilter}
                        sortBy={sortBy}
                        sortOrder={sortOrder}
                        onSort={handleSort}
                        uniqueOn={uniqueOn}
                        onUniqueChange={handleUniqueChange}
                    />
                    {paginationData.totalPages > 1 && (
                        <Pagination
                            currentPage={paginationData.currentPage}
                            totalPages={paginationData.totalPages}
                            onPageChange={handlePageChange}
                        />
                    )}
                </div>
            )}

            <div className="pt-8">
                <DetailCard title="Contact List Exporter">
                    <ContactExporter />
                </DetailCard>
            </div>
        </div>
    );
};

export default PhoneFilterPage;
```

### 5. Update Phone Filter Table (`src/components/PhoneFilterTable.tsx`)

Updated columns and handling for optional fields.

```tsx
// leads_webapp1/src/components/PhoneFilterTable.tsx
import React, { useId } from 'react';
import { Link } from 'react-router-dom';
import { PhoneFilterPerson } from '../types';

type SortKey = 'name' | 'age';
type SortOrder = 'asc' | 'desc';
type UniqueField = 'fullName' | 'formattedNumber' | 'standardNumber' | null;

interface PhoneFilterTableProps {
    people: PhoneFilterPerson[];
    nationalityFilter: string;
    onNationalityChange: (value: string) => void;
    ageFilter: { min: string, max: string };
    onAgeChange: (value: { min: string, max: string }) => void;
    sortBy: SortKey;
    sortOrder: SortOrder;
    onSort: (column: SortKey) => void;
    uniqueOn: UniqueField;
    onUniqueChange: (field: UniqueField | null) => void;
}

const SortIcon: React.FC<{ sortKey: SortKey, currentSortBy: SortKey, currentSortOrder: SortOrder }> = ({ sortKey, currentSortBy, currentSortOrder }) => {
    if (currentSortBy !== sortKey) return <svg className="h-4 w-4 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8 9l4-4 4 4m0 6l-4 4-4-4" /></svg>;
    if (currentSortOrder === 'asc') return <svg className="h-4 w-4 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M5 15l7-7 7 7" /></svg>;
    return <svg className="h-4 w-4 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 9l-7 7-7-7" /></svg>;
};

const UniqueToggle: React.FC<{ label: string; field: UniqueField; activeField: UniqueField | null; onChange: (field: UniqueField | null) => void; }> = ({ label, field, activeField, onChange }) => {
    const id = useId();
    const isChecked = activeField === field;
    const handleChange = () => {
        onChange(isChecked ? null : field);
    };

    return (
        <label htmlFor={id} className="flex items-center gap-1.5 cursor-pointer text-xs font-normal text-slate-500 dark:text-slate-400 hover:text-slate-800 dark:hover:text-white mt-1.5">
            <input
                id={id}
                type="checkbox"
                checked={isChecked}
                onChange={handleChange}
                className="h-3.5 w-3.5 rounded-sm border-slate-300 dark:border-slate-600 bg-slate-100 dark:bg-slate-700 text-primary focus:ring-primary focus:ring-offset-background-light dark:focus:ring-offset-background-dark"
            />
            <span>{label}</span>
        </label>
    );
};

export const PhoneFilterTable: React.FC<PhoneFilterTableProps> = ({
    people,
    nationalityFilter,
    onNationalityChange,
    ageFilter,
    onAgeChange,
    sortBy,
    sortOrder,
    onSort,
    uniqueOn,
    onUniqueChange,
}) => {
    return (
        <div className="bg-white dark:bg-slate-900 rounded-lg shadow-sm overflow-x-auto">
            <table className="w-full min-w-[1200px] text-left text-sm">
                <thead>
                    <tr className="bg-slate-50 dark:bg-slate-800 text-xs uppercase text-slate-500 dark:text-slate-400">
                        <th className="p-2 align-bottom">
                            <div onClick={() => onSort('name')} className="p-2 flex items-center gap-2 cursor-pointer hover:text-slate-800 dark:hover:text-white">
                                Name (Person/Entity)
                                <SortIcon sortKey="name" currentSortBy={sortBy} currentSortOrder={sortOrder} />
                            </div>
                            <div className="px-2 pb-2">
                                <UniqueToggle label="Unique Only" field="fullName" activeField={uniqueOn} onChange={onUniqueChange} />
                            </div>
                        </th>
                        <th className="p-2 align-bottom text-center">
                            <div onClick={() => onSort('age')} className="p-2 flex items-center justify-center gap-2 cursor-pointer hover:text-slate-800 dark:hover:text-white">
                                AGE
                                <SortIcon sortKey="age" currentSortBy={sortBy} currentSortOrder={sortOrder} />
                            </div>
                            <div className="px-2 pb-2 flex items-center justify-center gap-1">
                                <input type="number" placeholder="Min" value={ageFilter.min} onChange={(e) => onAgeChange({ ...ageFilter, min: e.target.value })} onClick={(e) => e.stopPropagation()} className="w-16 text-center bg-slate-100 dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded p-1 text-slate-800 dark:text-white focus:ring-primary focus:border-primary" />
                                <span>-</span>
                                <input type="number" placeholder="Max" value={ageFilter.max} onChange={(e) => onAgeChange({ ...ageFilter, max: e.target.value })} onClick={(e) => e.stopPropagation()} className="w-16 text-center bg-slate-100 dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded p-1 text-slate-800 dark:text-white focus:ring-primary focus:border-primary" />
                            </div>
                        </th>
                        <th className="p-4 align-bottom">Mobile Phone Number</th>
                        <th className="p-2 align-bottom">
                            <div className="p-2">NATIONALITY</div>
                            <div className="px-2 pb-2">
                                <input type="text" placeholder="Filter..." value={nationalityFilter} onChange={(e) => onNationalityChange(e.target.value)} onClick={(e) => e.stopPropagation()} className="w-full bg-slate-100 dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded p-1 text-slate-800 dark:text-white focus:ring-primary focus:border-primary" />
                            </div>
                        </th>
                        <th className="p-2 align-bottom">
                            <div className="p-2">Formatted Number</div>
                            <div className="px-2 pb-2">
                                <UniqueToggle label="Unique Only" field="formattedNumber" activeField={uniqueOn} onChange={onUniqueChange} />
                            </div>
                        </th>
                        <th className="p-2 align-bottom">
                            <div className="p-2">Standard Number</div>
                            <div className="px-2 pb-2">
                                <UniqueToggle label="Unique Only" field="standardNumber" activeField={uniqueOn} onChange={onUniqueChange} />
                            </div>
                        </th>
                    </tr>
                </thead>
                <tbody className="text-slate-600 dark:text-slate-300">
                    {people.length > 0 ? (
                        people.map((person, index) => {
                            const highlightedName = person.highlight?.full_name?.[0];
                            // Use optional check for age/nationality since they might be missing for entities
                            const isEntity = person.isEntity || (person.age === null && !person.nationality);

                            return (
                                <tr key={`${person.id}-${index}`} className="border-b border-slate-200 dark:border-slate-800 hover:bg-slate-50 dark:hover:bg-slate-800/50 transition-colors">
                                    <td className="p-4 font-semibold whitespace-nowrap text-slate-800 dark:text-white">
                                        {isEntity ? (
                                            <Link to={`/company/detail/${person.id}`} className="hover:text-primary hover:underline">
                                                 {highlightedName ? (<span dangerouslySetInnerHTML={{ __html: highlightedName }} />) : (person.fullName || 'N/A')}
                                            </Link>
                                        ) : (
                                            <Link to={`/person/${person.id}`} className="hover:text-primary hover:underline">
                                                {highlightedName ? (<span dangerouslySetInnerHTML={{ __html: highlightedName }} />) : (person.fullName || 'N/A')}
                                            </Link>
                                        )}
                                    </td>
                                    <td className="p-4 text-center">{person.age !== null ? person.age : <span className="text-slate-400">-</span>}</td>
                                    <td className="p-4">{person.mobilePhoneNumber || 'N/A'}</td>
                                    <td className="p-4">{person.nationality || <span className="text-slate-400">-</span>}</td>
                                    <td className="p-4">{person.formattedNumber || 'N/A'}</td>
                                    <td className="p-4">{person.standardNumber || 'N/A'}</td>
                                </tr>
                            )
                        })
                    ) : (
                        <tr>
                            <td colSpan={6} className="text-center py-16 px-4">
                                <div className="text-center py-16 px-4 bg-white dark:bg-slate-900 rounded-lg">
                                    <h2 className="text-xl font-semibold text-slate-800 dark:text-white">No Records Found</h2>
                                    <p className="text-slate-500 dark:text-gray-400 mt-2">Your search/filter criteria did not match any records with a phone number.</p>
                                </div>
                            </td>
                        </tr>
                    )}
                </tbody>
            </table>
        </div>
    );
};
```

### 6. Update Contact Exporter (`src/components/phone_filter/ContactExporter.tsx`)

Added the toggle for age/nationality filters.

```tsx
// leads_webapp1/src/components/phone_filter/ContactExporter.tsx
import React, { useState } from 'react';
import { ContactExportPreviewData } from '../../types';
import { ToggleSwitch } from '../ToggleSwitch';

const API_URL = import.meta.env.VITE_API_BASE_URL;

const InputField = ({ label, id, ...props }: { label: string; id: string; [key: string]: any }) => (
    <div>
        <label htmlFor={id} className="block text-sm font-medium text-slate-600 dark:text-slate-400 mb-1">
            {label}
        </label>
        <input id={id} className="w-full bg-slate-100 dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded p-2 text-slate-800 dark:text-white focus:ring-primary focus:border-primary disabled:opacity-50" {...props} />
    </div>
);

const LoadingSpinner = ({ message }: { message: string }) => (
    <div className="flex items-center gap-3 p-4 bg-blue-50 dark:bg-blue-900/20 border border-blue-200 dark:border-blue-800 rounded-lg">
        <svg className="animate-spin h-5 w-5 text-blue-600 dark:text-blue-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
            <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        <div className="flex-1">
            <p className="text-sm font-medium text-blue-900 dark:text-blue-200">{message}</p>
            <p className="text-xs text-blue-700 dark:text-blue-300 mt-1">This may take a moment for large datasets...</p>
        </div>
    </div>
);

export const ContactExporter: React.FC = () => {
    const [useFilters, setUseFilters] = useState(true);
    const [nationality, setNationality] = useState('Tanzanian');
    const [minAge, setMinAge] = useState('25');
    const [maxAge, setMaxAge] = useState('55');
    const [sampleSize, setSampleSize] = useState('5500');
    const [namePrefix, setNamePrefix] = useState('wapp_logo_CM21_');
    const [fileName, setFileName] = useState('contacts.csv');
    const [loading, setLoading] = useState(false);
    const [loadingMessage, setLoadingMessage] = useState('');
    const [error, setError] = useState<string | null>(null);
    const [previewData, setPreviewData] = useState<ContactExportPreviewData | null>(null);

    const getRequestBody = () => ({
        nationality: useFilters ? nationality : null, // Send null if filters disabled
        min_age: useFilters && minAge ? parseInt(minAge, 10) : null,
        max_age: useFilters && maxAge ? parseInt(maxAge, 10) : null,
        sample_size: parseInt(sampleSize, 10) || 0,
        name_prefix: namePrefix,
        file_name: fileName.endsWith('.csv') ? fileName : `${fileName}.csv`,
    });

    const handlePreview = async () => {
        setLoading(true);
        setLoadingMessage('Analyzing database and filtering contacts...');
        setError(null);
        setPreviewData(null);
        try {
            const response = await fetch(`${API_URL}/api/phone-filter/preview`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(getRequestBody()),
            });
            if (!response.ok) {
                const errData = await response.json();
                throw new Error(errData.detail || 'Failed to fetch preview data.');
            }
            const data = await response.json();
            setPreviewData(data);
        } catch (err: any) {
            setError(err.message);
        } finally {
            setLoading(false);
            setLoadingMessage('');
        }
    };

    const handleDownload = async () => {
        setLoading(true);
        setLoadingMessage('Generating your export file...');
        setError(null);
        try {
            const response = await fetch(`${API_URL}/api/phone-filter/download`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(getRequestBody()),
            });
            if (!response.ok) {
                const errData = await response.json();
                throw new Error(errData.detail || 'Failed to download file.');
            }
            const blob = await response.blob();
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = fileName.endsWith('.csv') ? fileName : `${fileName}.csv`;
            document.body.appendChild(a);
            a.click();
            a.remove();
            window.URL.revokeObjectURL(url);
        } catch (err: any) {
            setError(err.message);
        } finally {
            setLoading(false);
            setLoadingMessage('');
        }
    };

    return (
        <div className="space-y-4">
            <div className="flex items-center gap-4 mb-2">
                <ToggleSwitch label="Filter by Demographics (Age/Nationality)" enabled={useFilters} onChange={setUseFilters} />
                {!useFilters && <span className="text-xs text-slate-500 italic">(Exports all matching records including entities)</span>}
            </div>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                <InputField label="Nationality" id="nationality" value={nationality} onChange={(e: any) => setNationality(e.target.value)} disabled={!useFilters} />
                <InputField label="Min Age" id="min-age" type="number" value={minAge} onChange={(e: any) => setMinAge(e.target.value)} disabled={!useFilters} />
                <InputField label="Max Age" id="max-age" type="number" value={maxAge} onChange={(e: any) => setMaxAge(e.target.value)} disabled={!useFilters} />
                <InputField label="Sample Size" id="sample-size" type="number" value={sampleSize} onChange={(e: any) => setSampleSize(e.target.value)} />
                <InputField label="Contact Name Prefix" id="name-prefix" value={namePrefix} onChange={(e: any) => setNamePrefix(e.target.value)} />
                <InputField label="Download Filename (.csv)" id="filename" value={fileName} onChange={(e: any) => setFileName(e.target.value)} />
            </div>

            {loading && <LoadingSpinner message={loadingMessage} />}

            <div className="flex items-center gap-4 pt-2">
                <button onClick={handlePreview} disabled={loading} className="px-4 py-2 bg-blue-500 text-white rounded-md disabled:bg-slate-500 disabled:cursor-not-allowed hover:bg-blue-600 transition-colors" >
                    {loading && loadingMessage.includes('Analyzing') ? 'Processing...' : 'Preview'}
                </button>
                <button onClick={handleDownload} disabled={loading} className="px-4 py-2 bg-green-500 text-white rounded-md disabled:bg-slate-500 disabled:cursor-not-allowed hover:bg-green-600 transition-colors" >
                    {loading && loadingMessage.includes('Generating') ? 'Downloading...' : 'Download'}
                </button>
            </div>

            {error && (
                <div className="p-4 bg-red-500/10 border border-red-500/50 rounded-md">
                    <p className="text-red-700 dark:text-red-400 font-medium">Error</p>
                    <p className="text-red-600 dark:text-red-500 text-sm mt-1">{error}</p>
                </div>
            )}

            {previewData && (
                <div className="space-y-4 pt-4 border-t border-slate-700">
                    <h4 className="text-lg font-semibold text-white">Preview Results</h4>
                    <ul className="list-disc list-inside space-y-1 text-slate-300">
                        <li>Initial records matching criteria: <strong>{previewData.stats.initial_count.toLocaleString()}</strong></li>
                        <li>Duplicate phone numbers removed: <strong>{previewData.stats.duplicates_removed.toLocaleString()}</strong></li>
                        <li>Already engaged numbers removed: <strong>{previewData.stats.engaged_removed.toLocaleString()}</strong></li>
                        <li>Final unique, clean records: <strong>{previewData.stats.final_count.toLocaleString()}</strong></li>
                        <li>Final sample size for download: <strong>{previewData.stats.sample_size.toLocaleString()}</strong></li>
                    </ul>
                    <h5 className="font-semibold pt-2 text-white">Sample Data from Generated File</h5>
                    <div className="overflow-x-auto">
                        <table className="w-full text-left text-sm">
                            <thead className="text-xs text-slate-400 uppercase bg-slate-800">
                                <tr>
                                    <th className="p-2">Name</th>
                                    <th className="p-2">Phone Number</th>
                                </tr>
                            </thead>
                            <tbody className="text-slate-300">
                                {previewData.sample_data_head.map(row => (
                                    <tr key={row.Name} className="border-b border-slate-700">
                                        <td className="p-2">{row.Name}</td>
                                        <td className="p-2">{row['Phone Number']}</td>
                                    </tr>
                                ))}
                                {previewData.sample_data_tail.length > 0 && (
                                    <tr>
                                        <td colSpan={2} className="text-center py-2 text-slate-500">...</td>
                                    </tr>
                                )}
                                {previewData.sample_data_tail.map(row => (
                                    <tr key={row.Name} className="border-b border-slate-700">
                                        <td className="p-2">{row.Name}</td>
                                        <td className="p-2">{row['Phone Number']}</td>
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
```

### 7. Update Elasticsearch Client (`backend_python/app/elasticsearch_client.py`)

Added mappings for `mobilePhoneNumberBsness` and `emailAddressBsness` in the `business_place_properties`.

```python
# backend_python/app/elasticsearch_client.py
from elasticsearch import Elasticsearch, AsyncElasticsearch
from elasticsearch.helpers import bulk, async_bulk
from typing import Dict, Any, List, Optional
import logging
from .config import settings
import json

logger = logging.getLogger(__name__)

class ElasticsearchClient:
    """Elasticsearch client wrapper with connection management"""

    def __init__(self):
        self.client: Optional[Elasticsearch] = None
        self.async_client: Optional[AsyncElasticsearch] = None

    def connect(self):
        """Establish synchronous connection to Elasticsearch"""
        try:
            self.client = Elasticsearch(
                [settings.elasticsearch_url],
                basic_auth=(settings.elasticsearch_user, settings.elasticsearch_password),
                verify_certs=False,
                ssl_show_warn=False,
                request_timeout=30,
                max_retries=3,
                retry_on_timeout=True
            )
            if self.client.ping():
                logger.info(f"Connected to Elasticsearch at {settings.elasticsearch_url}")
            else:
                raise ConnectionError("Cannot connect to Elasticsearch")
        except Exception as e:
            logger.error(f"Elasticsearch connection error: {str(e)}")
            raise

    def connect_async(self):
        """Establish asynchronous connection to Elasticsearch"""
        try:
            self.async_client = AsyncElasticsearch(
                [settings.elasticsearch_url],
                basic_auth=(settings.elasticsearch_user, settings.elasticsearch_password),
                verify_certs=False,
                ssl_show_warn=False,
                request_timeout=30,
                max_retries=3,
                retry_on_timeout=True
            )
            logger.info(f"Async client configured for Elasticsearch at {settings.elasticsearch_url}")
        except Exception as e:
            logger.error(f"Elasticsearch async connection error: {str(e)}")
            raise

    def close(self):
        if self.client:
            self.client.close()
            logger.info("Elasticsearch connection closed")

    async def close_async(self):
        if self.async_client:
            await self.async_client.close()
            logger.info("Elasticsearch async connection closed")

    def create_indices(self):
        """Create Elasticsearch indices with mappings"""
        if not self.client:
            raise ConnectionError("Synchronous client not connected.")

        # --- Define reusable property mappings ---
        applicant_properties = {
            "emailAddress": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "mobilePhoneNumber": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "mobilePhoneNumber_normalized": {"type": "keyword"},
            "approvalStatus": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "nationalID": {"type": "keyword"},
            "passportNumber": {"type": "keyword"},
            "nida_region": {"type": "keyword"},
            "nida_district": {"type": "keyword"},
            "nida_ward": {"type": "keyword"}
        }

        person_nested_properties = {
            "emailAddress": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "mobilePhoneNumber": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "mobilePhoneNumber_normalized": {"type": "keyword"},
            "nationalId": {"type": "keyword"},
            "nationalID": {"type": "keyword"},
            "passportNumber": {"type": "keyword"},
            "shareholderId": {"type": "keyword"},
            "type": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "country": {"type": "keyword"},
            "lastName": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
            "firstName": {"type": "text"},
            "middleName": {"type": "text"},
            "dateOfBirth": {"type": "keyword"},
            "nationality": {"type": "keyword"},
            "nida_region": {"type": "keyword"},
            "nida_district": {"type": "keyword"},
            "nida_ward": {"type": "keyword"},
            "shares_count": {"type": "long"},
            "share_value": {"type": "double"}
        }

        autocomplete_analyzer = {
            "analysis": {
                "filter": {
                    "autocomplete_filter": {
                        "type": "edge_ngram",
                        "min_gram": 2,
                        "max_gram": 20
                    }
                },
                "analyzer": {
                    "autocomplete_analyzer": {
                        "type": "custom",
                        "tokenizer": "standard",
                        "filter": [
                            "lowercase",
                            "autocomplete_filter"
                        ]
                    }
                }
            }
        }

        business_place_properties = {
            "type": "object",
            "properties": {
                "region": {"type": "keyword"},
                "district": {"type": "keyword"},
                "ward": {"type": "keyword"},
                "streetBusiness": {"type": "keyword"},
                "roadBusiness": {"type": "keyword"},
                "typeOfLocalAddress": {"type": "keyword"},
                "mobilePhoneNumberBsness": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
                "emailAddressBsness": {"type": "text", "fields": {"keyword": {"type": "keyword"}}}
            }
        }

        # --- Company Index Mapping ---
        companies_mapping = {
            "mappings": {
                "properties": {
                    "id": {"type": "keyword"},
                    "company_name": {"type": "text", "fields": {"keyword": {"type": "keyword"}, "suggest": {"type": "completion"}, "autocomplete": {"type": "text", "analyzer": "autocomplete_analyzer", "search_analyzer": "standard"}}},
                    "registration_type": {"type": "keyword"},
                    "incorporation_number": {"type": "keyword"},
                    "tin": {"type": "keyword"},
                    "incorporation_date": {"type": "keyword"},
                    "applicant_info": {"type": "object", "properties": applicant_properties},
                    "business_place": business_place_properties,
                    "company_secretary_detail": {"type": "object", "properties": person_nested_properties},
                    "other_info": {"type": "object", "enabled": False},
                    "director_details": {"type": "nested", "properties": person_nested_properties},
                    "shareholder_details": {"type": "nested", "properties": person_nested_properties},
                    "business_activities": {"type": "keyword"},
                    "created_at": {"type": "date"},
                    "authorised_share": {"type": "long"},
                    "time_application_created": {"type": "keyword"},
                    "time_application_submitted": {"type": "keyword"},
                    "service_type": {"type": "keyword"}
                }
            },
            "settings": {
                "number_of_shards": 1,
                "number_of_replicas": 1,
                "index.mapping.total_fields.limit": 2000,
                "index.max_result_window": 3000000,
                **autocomplete_analyzer
            }
        }

        # --- Business Name Index Mapping ---
        business_names_mapping = {
            "mappings": {
                "properties": {
                    "id": {"type": "keyword"},
                    "business_name": {"type": "text", "fields": {"keyword": {"type": "keyword"}, "suggest": {"type": "completion"}, "autocomplete": {"type": "text", "analyzer": "autocomplete_analyzer", "search_analyzer": "standard"}}},
                    "registration_type": {"type": "keyword"},
                    "registration_number": {"type": "keyword"},
                    "registration_date": {"type": "keyword"},
                    "business_type": {"type": "keyword"},
                    "applicant_info": {"type": "object", "properties": applicant_properties},
                    "business_place": business_place_properties,
                    "owner_details": {"type": "nested", "properties": person_nested_properties},
                    "persons_who_can_update_details": {"type": "nested", "properties": person_nested_properties},
                    "bank_operator_details": {"type": "nested", "properties": person_nested_properties},
                    "business_activities": {"type": "keyword"},
                    "created_at": {"type": "date"},
                    "time_application_created": {"type": "keyword"}
                }
            },
            "settings": {
                "number_of_shards": 1,
                "number_of_replicas": 1,
                "index.mapping.total_fields.limit": 2000,
                "index.max_result_window": 3000000,
                **autocomplete_analyzer
            }
        }

        # --- People Index Mapping (Unified) ---
        people_mapping = {
            "mappings": {
                "properties": {
                    "person_identifier": {"type": "keyword"},
                    "full_name": {"type": "text", "fields": {"keyword": {"type": "keyword"}, "suggest": {"type": "completion"}, "autocomplete": {"type": "text", "analyzer": "autocomplete_analyzer", "search_analyzer": "standard"}}},
                    "date_of_birth": {"type": "date", "format": "yyyy-MM-dd||dd/MM/yyyy||epoch_millis"},
                    "nationality": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
                    "role": {"type": "keyword"},
                    "registration_id": {"type": "keyword"},
                    "source_record_id": {"type": "keyword"},
                    "registration_name": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
                    "registration_type": {"type": "keyword"},
                    "approval_status": {"type": "keyword"},
                    "record_created_at": {"type": "date", "format": "dd/MM/yyyy HH:mm:ss"},
                    "details": {"type": "object", "properties": person_nested_properties}
                }
            },
            "settings": {
                "number_of_shards": 1,
                "number_of_replicas": 1,
                "index.mapping.total_fields.limit": 2000,
                "index.max_result_window": 3000000,
                **autocomplete_analyzer
            }
        }
        
        # --- NEW: Corporate Shareholder Index Mapping ---
        corporate_shareholders_mapping = {
            "mappings": {
                "properties": {
                    "id": {"type": "keyword"},
                    "name": {"type": "text", "fields": {"keyword": {"type": "keyword"}, "autocomplete": {"type": "text", "analyzer": "autocomplete_analyzer", "search_analyzer": "standard"}}},
                    "country": {"type": "keyword"},
                    "email_address": {"type": "keyword"},
                    "phone_number": {"type": "keyword"},
                    "phone_number_normalized": {"type": "keyword"},
                    "shareholdings": {
                        "type": "nested",
                        "properties": {
                            "company_id": {"type": "keyword"},
                            "company_name": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
                            "shares_class": {"type": "keyword"},
                            "shares_count": {"type": "long"}
                        }
                    },
                    "shareholding_count": {"type": "integer"},
                    "created_at": {"type": "date"}
                }
            },
            "settings": {
                "number_of_shards": 1,
                "number_of_replicas": 1,
                "index.mapping.total_fields.limit": 2000,
                "index.max_result_window": 3000000,
                **autocomplete_analyzer
            }
        }

        # --- NEW: TRA TINS Index Mapping ---
        tra_tins_mapping = {
            "mappings": {
                "properties": {
                    "id": {"type": "keyword"},
                    "tin_no": {"type": "keyword"},
                    "name": {
                        "type": "text",
                        "fields": {
                            "keyword": {"type": "keyword"},
                            "autocomplete": {"type": "text", "analyzer": "autocomplete_analyzer", "search_analyzer": "standard"}
                        }
                    },
                    "firstname": {"type": "text"},
                    "middlename": {"type": "text"},
                    "lastname": {"type": "text"},
                    "tin_type": {"type": "keyword"},
                    "national_id": {"type": "keyword"},
                    "incorporation_number": {"type": "keyword"},
                    "telephones": {"type": "keyword"},
                    "telephones_normalized": {"type": "keyword"},
                    "business_start_date": {"type": "date", "format": "M/d/yyyy H:mm:ss a||yyyy-MM-dd||epoch_millis", "ignore_malformed": True},
                    "pobox": {"type": "keyword"},
                    "postal_city": {"type": "keyword"},
                    "created_at": {"type": "date"},
                    "date_of_birth": {"type": "date", "format": "yyyy-MM-dd", "ignore_malformed": True},
                    "age": {"type": "integer"},
                    "nida_region": {"type": "keyword"},
                    "nida_district": {"type": "keyword"},
                    "nida_ward": {"type": "keyword"}
                }
            },
            "settings": {
                "number_of_shards": 1,
                "number_of_replicas": 1,
                "index.max_result_window": 3000000,
                **autocomplete_analyzer
            }
        }
        
        # --- NEW: Wabunge Index Mapping ---
        wabunge_mapping = {
            "mappings": {
                "properties": {
                    "id": {"type": "keyword"},
                    "name": {
                        "type": "text",
                        "fields": {
                            "keyword": {"type": "keyword"},
                            "autocomplete": {"type": "text", "analyzer": "autocomplete_analyzer", "search_analyzer": "standard"}
                        }
                    },
                    "title": {"type": "keyword"},
                    "phone1": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
                    "phone1_normalized": {"type": "keyword"},
                    "phone2": {"type": "text", "fields": {"keyword": {"type": "keyword"}}},
                    "phone2_normalized": {"type": "keyword"},
                    "chama": {"type": "keyword"},
                    "address1": {"type": "text"},
                    "address2": {"type": "text"},
                    "created_at": {"type": "date"}
                }
            },
            "settings": {
                "number_of_shards": 1,
                "number_of_replicas": 1,
                "index.max_result_window": 10000,
                **autocomplete_analyzer
            }
        }

        indices_to_create = {
            "companies": companies_mapping,
            "business_names": business_names_mapping,
            "people": people_mapping,
            "corporate_shareholders": corporate_shareholders_mapping,
            "tra_tins": tra_tins_mapping,
            "wabunge": wabunge_mapping
        }

        for index_name, mapping in indices_to_create.items():
            if self.client.indices.exists(index=index_name):
                self.client.indices.delete(index=index_name)
                logger.info(f"Deleted existing '{index_name}' index.")
            
            self.client.indices.create(index=index_name, body=mapping)
            logger.info(f"Created '{index_name}' index")

es_client = ElasticsearchClient()
```

### 8. Update Search Logic (`backend_python/app/routes/search.py`)

Included logic to check the `business_place` object for matches.

```python
# backend_python/app/routes/search.py
from fastapi import APIRouter, HTTPException, Path, Query
import logging
from typing import Dict, Any
from ..elasticsearch_client import es_client
from ..models import ErrorResponse, ContactSearchResponse
from ..utils.helpers import normalize_phone_number

router = APIRouter(prefix="/api", tags=["search"])
logger = logging.getLogger(__name__)

def process_company_hit(hit: Dict[str, Any], contact_value: str) -> Dict[str, Any]:
    source = hit.get("_source", {})
    found_in_sections = set()

    applicant_info = source.get("applicant_info", {})
    if applicant_info:
        if applicant_info.get("emailAddress") == contact_value:
            found_in_sections.add("Applicant")
        if normalize_phone_number(applicant_info.get("mobilePhoneNumber")) == normalize_phone_number(contact_value):
            found_in_sections.add("Applicant")
            
    secretary_detail = source.get("company_secretary_detail", {})
    if secretary_detail:
        if secretary_detail.get("emailAddress") == contact_value:
            found_in_sections.add("Secretary")
        if normalize_phone_number(secretary_detail.get("mobilePhoneNumber")) == normalize_phone_number(contact_value):
            found_in_sections.add("Secretary")

    # NEW: Check Place of Business
    business_place = source.get("business_place", {})
    if business_place:
        # Check various possible keys for emails
        if business_place.get("emailAddressBsness") == contact_value: found_in_sections.add("Place of Business (Email)")
        if business_place.get("emailAddressCmpny") == contact_value: found_in_sections.add("Place of Business (Email)")
        
        # Check various possible keys for phones
        norm_contact = normalize_phone_number(contact_value)
        if normalize_phone_number(business_place.get("mobilePhoneNumberBsness")) == norm_contact: found_in_sections.add("Place of Business (Phone)")
        if normalize_phone_number(business_place.get("mobilePhoneNumberCmpny")) == norm_contact: found_in_sections.add("Place of Business (Phone)")

    inner_hits = hit.get("inner_hits", {})
    if "director_details" in inner_hits and inner_hits["director_details"]["hits"]["total"]["value"] > 0:
        found_in_sections.add("Director")
    if "shareholder_details" in inner_hits and inner_hits["shareholder_details"]["hits"]["total"]["value"] > 0:
        found_in_sections.add("Shareholder")
        
    return {
        "companyId": source.get("id"), 
        "companyName": source.get("company_name"),
        "foundInSections": ", ".join(sorted(list(found_in_sections))),
        "approvalStatus": applicant_info.get("approvalStatus", "Unknown")
    }

def process_business_name_hit(hit: Dict[str, Any], contact_value: str) -> Dict[str, Any]:
    source = hit.get("_source", {})
    found_in_sections = set()

    applicant_info = source.get("applicant_info", {})
    if applicant_info:
        if applicant_info.get("emailAddress") == contact_value:
            found_in_sections.add("Applicant")
        if normalize_phone_number(applicant_info.get("mobilePhoneNumber")) == normalize_phone_number(contact_value):
            found_in_sections.add("Applicant")
    
    # NEW: Check Place of Business
    business_place = source.get("business_place", {})
    if business_place:
         if business_place.get("emailAddressBsness") == contact_value: found_in_sections.add("Place of Business (Email)")
         norm_contact = normalize_phone_number(contact_value)
         if normalize_phone_number(business_place.get("mobilePhoneNumberBsness")) == norm_contact: found_in_sections.add("Place of Business (Phone)")

    inner_hits = hit.get("inner_hits", {})
    if "owner_details" in inner_hits and inner_hits["owner_details"]["hits"]["total"]["value"] > 0:
        found_in_sections.add("Owner")
    if "persons_who_can_update_details" in inner_hits and inner_hits["persons_who_can_update_details"]["hits"]["total"]["value"] > 0:
        found_in_sections.add("Authorized Person")
    if "bank_operator_details" in inner_hits and inner_hits["bank_operator_details"]["hits"]["total"]["value"] > 0:
        found_in_sections.add("Bank Operator")

    return {
        "businessNameId": source.get("id"),
        "businessName": source.get("business_name"),
        "foundInSections": ", ".join(sorted(list(found_in_sections))),
        "approvalStatus": applicant_info.get("approvalStatus", "Unknown")
    }

def process_person_hit(hit: Dict[str, Any]) -> Dict[str, Any]:
    source = hit.get("_source", {})
    return {
        "personId": source.get("person_identifier"),
        "fullName": source.get("full_name"),
        "role": source.get("role"),
        "registrationId": source.get("registration_id"),
        "registrationName": source.get("registration_name"),
        "registrationType": source.get("registration_type"),
        "approvalStatus": source.get("approval_status", "Unknown")
    }

@router.get("/search/contact/{contact_value}", response_model=ContactSearchResponse, responses={500: {"model": ErrorResponse}})
async def search_by_contact(
    contact_value: str = Path(..., description="Email address or phone number to search for"),
    approvedOnly: bool = Query(False, description="Filter results for approved registrations only")
):
    try:
        normalized_phone = normalize_phone_number(contact_value)
        is_email = "@" in contact_value
        
        email_clauses = lambda field_prefix: [{"term": {f"{field_prefix}.emailAddress.keyword": contact_value}}]
        phone_clauses = lambda field_prefix: [{"term": {f"{field_prefix}.mobilePhoneNumber_normalized": normalized_phone}}] if normalized_phone else [{"term": {f"{field_prefix}.mobilePhoneNumber.keyword": contact_value}}]
        
        def get_clauses(field_prefix):
            return email_clauses(field_prefix) if is_email else phone_clauses(field_prefix)

        def get_nested_clauses(path):
            return {"nested": {"path": path, "query": {"bool": {"should": get_clauses(path), "minimum_should_match": 1}}, "inner_hits": {}}}
            
        # --- Base Filter ---
        company_bn_filter = [{"term": {"applicant_info.approvalStatus.keyword": "Approve"}}] if approvedOnly else []
        people_filter = [{"term": {"approval_status.keyword": "Approve"}}] if approvedOnly else []

        # --- Business Place Logic ---
        # Logic to search the flat object business_place
        def get_business_place_clauses():
             if is_email:
                 return [
                     {"term": {"business_place.emailAddressBsness.keyword": contact_value}},
                     {"term": {"business_place.emailAddressCmpny.keyword": contact_value}}
                 ]
             else:
                 # Since we don't have a normalized field in business_place guaranteed, we wildcard search or exact match
                 # This is a best-effort match for entity phones
                 return [
                     {"term": {"business_place.mobilePhoneNumberBsness.keyword": contact_value}},
                     {"term": {"business_place.mobilePhoneNumberCmpny.keyword": contact_value}}
                 ]

        # --- Company Query ---
        company_should_clauses = get_clauses("applicant_info") + get_clauses("company_secretary_detail") + get_business_place_clauses()
        company_should_clauses.extend([
            get_nested_clauses("director_details"),
            get_nested_clauses("shareholder_details")
        ])
        company_query = {
            "query": {"bool": {"must": company_bn_filter, "should": company_should_clauses, "minimum_should_match": 1}},
            "_source": ["id", "company_name", "applicant_info", "incorporation_number", "business_place", "company_secretary_detail"],
            "size": 100
        }

        # --- Business Name Query ---
        bn_should_clauses = get_clauses("applicant_info") + get_business_place_clauses()
        bn_should_clauses.extend([
            get_nested_clauses("owner_details"),
            get_nested_clauses("persons_who_can_update_details"),
            get_nested_clauses("bank_operator_details")
        ])
        bn_query = {
            "query": {"bool": {"must": company_bn_filter, "should": bn_should_clauses, "minimum_should_match": 1}},
            "_source": ["id", "business_name", "applicant_info", "business_place"],
            "size": 100
        }

        # --- People Query ---
        people_query = {
             "query": {"bool": {"must": people_filter, "should": get_clauses("details"), "minimum_should_match": 1}},
             "size": 100
        }
        
        # --- Associated TINs Query ---
        tins_query = {"query": {"match_none": {}}}
        if not is_email and normalized_phone:
             tins_query = {
                 "query": {"term": {"telephones_normalized": normalized_phone}},
                 "size": 50
             }

        # --- MSearch ---
        msearch_body = [
            {"index": "companies"}, company_query,
            {"index": "business_names"}, bn_query,
            {"index": "people"}, people_query,
            {"index": "tra_tins"}, tins_query
        ]

        msearch_result = await es_client.async_client.msearch(body=msearch_body)
        
        company_results_raw = msearch_result["responses"][0]
        bn_results_raw = msearch_result["responses"][1]
        people_results_raw = msearch_result["responses"][2]
        tins_results_raw = msearch_result["responses"][3]

        companies = [process_company_hit(hit, contact_value) for hit in company_results_raw.get("hits", {}).get("hits", [])]
        business_names = [process_business_name_hit(hit, contact_value) for hit in bn_results_raw.get("hits", {}).get("hits", [])]
        people = [process_person_hit(hit) for hit in people_results_raw.get("hits", {}).get("hits", [])]
        
        associated_tins = []
        for hit in tins_results_raw.get("hits", {}).get("hits", []):
            source = hit["_source"]
            associated_tins.append({
                "id": source.get("id"),
                "tinNo": source.get("tin_no"),
                "name": source.get("name"),
                "tinType": source.get("tin_type"),
                "postalCity": source.get("postal_city"),
                "businessStartDate": source.get("business_start_date"),
                "telephones": source.get("telephones", [])
            })

        return ContactSearchResponse(
            success=True,
            companies=companies,
            business_names=business_names,
            people=people,
            associatedTins=associated_tins
        )

    except Exception as e:
        logger.error(f"Error searching by contact '{contact_value}': {str(e)}", exc_info=True)
        raise HTTPException(status_code=500, detail="An internal error occurred during search.")
```

### 9. Update Phone Filter Utils (`backend_python/app/routes/phone_filter/utils.py`)

Updated to handle transformation of both people and entity documents.

```python
# backend_python/app/routes/phone_filter/utils.py
import json
import base64
from typing import Optional, List, Dict, Any
from datetime import datetime
from ...utils.helpers import format_mobile_number, format_standard_number

def encode_search_after(sort_values: List) -> str:
    """Encodes Elasticsearch sort values for cursor-based pagination."""
    try:
        json_str = json.dumps(sort_values)
        return base64.b64encode(json_str.encode()).decode()
    except Exception:
        return ""

def decode_search_after(encoded: str) -> Optional[List]:
    """Decodes a cursor into Elasticsearch sort values."""
    try:
        if not encoded:
            return None
        json_str = base64.b64decode(encoded.encode()).decode()
        return json.loads(json_str)
    except Exception:
        return None

def calculate_age(dob_str: Optional[str]) -> Optional[int]:
    """Calculates age from a date string (YYYY-MM-DD)."""
    if not dob_str:
        return None
    try:
        birth_date = datetime.strptime(dob_str.split('T')[0], "%Y-%m-%d")
        today = datetime.today()
        age = today.year - birth_date.year - ((today.month, today.day) < (birth_date.month, birth_date.day))
        return age
    except (ValueError, TypeError):
        return None

def transform_person_for_phone_filter(doc: Dict[str, Any]) -> Dict[str, Any]:
    """
    Transforms an Elasticsearch document into the format for the PhoneFilterPerson model.
    Handles both People documents and Entity (Company/Business Name) documents.
    """
    source = doc.get("_source", {})
    index = doc.get("_index")
    
    id_val = source.get("id") or source.get("person_identifier")
    full_name = "Unknown"
    mobile_number = None
    nationality = None
    age = None
    
    if index == "people":
        full_name = source.get("full_name")
        mobile_number = source.get("details", {}).get("mobilePhoneNumber")
        nationality = source.get("nationality")
        age = calculate_age(source.get("date_of_birth"))
    else: 
        # It's a company or business name
        full_name = source.get("company_name") or source.get("business_name")
        business_place = source.get("business_place", {})
        # Try different keys for phone number in business_place
        mobile_number = business_place.get("mobilePhoneNumberBsness") or \
                        business_place.get("mobilePhoneNumberCmpny")
        
        # Entities generally don't have age or nationality in this context
        nationality = None 
        age = None

    return {
        "id": id_val,
        "fullName": full_name,
        "age": age,
        "mobilePhoneNumber": mobile_number,
        "nationality": nationality,
        "formattedNumber": format_mobile_number(mobile_number),
        "standardNumber": format_standard_number(mobile_number),
        "highlight": doc.get("highlight", {})
    }
```

### 10. Update Phone Filter Router (`backend_python/app/routes/phone_filter/router.py`)

Updated to query across multiple indices and handle pagination correctly with the new structure.

```python
# backend_python/app/routes/phone_filter/router.py
import logging
from typing import Optional
from fastapi import APIRouter, Query, HTTPException, BackgroundTasks
from fastapi.responses import StreamingResponse
from ...models import (
    PhoneFilterListResponse, ErrorResponse, ContactExportRequest,
    ContactExportPreviewResponse, PaginationResponse
)
from ...elasticsearch_client import es_client
from ...utils.helpers import build_es_query, calculate_age_range
from . import services, cache, utils

router = APIRouter(prefix="/api/phone-filter", tags=["phone-filter"])
logger = logging.getLogger(__name__)

@router.get("", response_model=PhoneFilterListResponse, responses={500: {"model": ErrorResponse}})
async def get_phone_filter_data(
    page: int = Query(1, ge=1),
    limit: int = Query(10, ge=1, le=100),
    registration_type: Optional[str] = Query(None, description="Optional: 'company' or 'business_name'"),
    search: Optional[str] = Query(None, description="Search by name"),
    wholeWord: bool = Query(False),
    wholeSentence: bool = Query(False),
    nationality: Optional[str] = Query(None, description="Filter by nationality"),
    minAge: Optional[int] = Query(None, ge=0),
    maxAge: Optional[int] = Query(None, ge=0),
    sortBy: str = Query("name", description="Sort field: name, age"),
    sortOrder: str = Query("asc", description="Sort order"),
    searchAfter: Optional[str] = Query(None),
    unique_on_field: Optional[str] = Query(None, description="Collapse results on this field to get unique values.")
):
    try:
        # --- INDEX SELECTION ---
        # If Age or Nationality filters are present, we essentially restrict to 'people'
        # because companies don't typically have these fields.
        # Otherwise, we search people, companies, and business names.
        indices = "people,companies,business_names"
        
        # --- QUERY CONSTRUCTION ---
        should_conditions = [
            {"exists": {"field": "details.mobilePhoneNumber"}}, # For People
            {"exists": {"field": "business_place.mobilePhoneNumberBsness"}}, # For Entities
            {"exists": {"field": "business_place.mobilePhoneNumberCmpny"}}   # For Entities fallback
        ]
        
        must_conditions = [
            {"bool": {"minimum_should_match": 1, "should": should_conditions}}
        ]

        if registration_type:
             must_conditions.append({"term": {"registration_type": registration_type}})

        if nationality:
            must_conditions.append({"match": {"nationality": {"query": nationality, "fuzziness": "AUTO"}}})
            # If filtering by nationality, effectively only People will return.
            
        age_range = calculate_age_range(minAge, maxAge)
        if age_range:
            must_conditions.append({"range": {"date_of_birth": age_range}})
            # If filtering by age, effectively only People will return.

        search_fields = ["full_name^3", "company_name^3", "business_name^3"]
        query = build_es_query(
            must_conditions=must_conditions,
            search_term=search,
            search_fields=search_fields,
            whole_word=wholeWord,
            whole_sentence=wholeSentence
        )

        # --- SORT LOGIC ---
        # Sorting across multiple indices with different field names is complex.
        # We use a script sort or `coalesce` logic if possible, or just sort by score if searching.
        # Simplification: If "name", sort on full_name AND company_name/business_name if possible using unmapped_type.
        
        sort_order_val = sortOrder.lower()
        sort_config = []
        
        if sortBy == "age":
            sort_order_val = "desc" if sortOrder.lower() == "asc" else "asc"
            # Sort by date_of_birth. Companies without it will be last.
            sort_config.append({"date_of_birth": {"order": sort_order_val, "missing": "_last", "unmapped_type": "long"}})
        else:
            # Sort by name. We try to sort by full_name.keyword, company_name.keyword, etc.
            # ES allows multiple fields in sort, but it takes the first one that exists.
            # For a mixed index search, strictly sorting alphabetically across different fields requires a mapped alias.
            # Fallback: We sort by score if searching, otherwise creation date.
            if search:
                sort_config.append("_score")
            else:
                 # Use created_at or similar common field if possible, or default to ID
                 sort_config.append({"_id": {"order": "asc"}})

        # If no specific sort, add tie-breaker
        sort_config.append({"_doc": {"order": "asc"}})

        search_params = {
            "index": indices,
            "query": query,
            "size": limit,
            "sort": sort_config,
            "track_total_hits": True,
            # Source filtering to reduce payload
            "_source": [
                "person_identifier", "full_name", "date_of_birth", "nationality", "details.mobilePhoneNumber",
                "id", "company_name", "business_name", "business_place.mobilePhoneNumberBsness", "business_place.mobilePhoneNumberCmpny"
            ],
            "highlight": {
                "fields": {
                    "full_name": {},
                    "company_name": {},
                    "business_name": {}
                }
            }
        }

        total_count = 0

        # --- PAGINATION & COLLAPSE ---
        # Unique collapsing is tricky across mixed indices with different field names.
        # For this implementation, we will restrict 'unique_on_field' usage to scenarios where it maps cleanly,
        # or disable it for mixed results to prevent errors.
        
        if unique_on_field:
             # Mapping frontend field to backend field(s).
             # Since this is complex across indices, we might restrict unique filter to 'people' index for safety,
             # OR we perform a simplified search.
             # For now, if unique is requested, we default to standard pagination logic but warn/limit scope.
             if unique_on_field == "fullName":
                 # Companies have company_name.keyword, People have full_name.keyword
                 pass 
             elif unique_on_field == "formattedNumber":
                 pass
             
             # For stability in this specific mixed-entity request, we disable ES collapsing 
             # unless we are sure about the field mapping. 
             # Instead, we stick to standard search params.
             pass

        # Standard pagination
        if page > 1 and searchAfter:
            search_params["search_after"] = utils.decode_search_after(searchAfter)
        else:
            search_params["from_"] = (page - 1) * limit

        result = await es_client.async_client.search(**search_params)
        people = [utils.transform_person_for_phone_filter(hit) for hit in result["hits"]["hits"]]
        
        total_count = result["hits"]["total"]["value"]
        total_pages = (total_count + limit - 1) // limit if limit > 0 else 0

        next_search_after = None
        if people and "sort" in result["hits"]["hits"][-1]:
             next_search_after = utils.encode_search_after(result["hits"]["hits"][-1]["sort"])

        pagination_data = PaginationResponse(
            currentPage=page,
            totalPages=total_pages,
            totalCount=total_count,
            limit=limit,
            searchAfter=next_search_after,
            hasMore=page < total_pages
        )

        return PhoneFilterListResponse(success=True, people=people, pagination=pagination_data)

    except Exception as e:
        logger.error(f"Error fetching phone filter data: {str(e)}", exc_info=True)
        raise HTTPException(
            status_code=500,
            detail="An internal error occurred while fetching phone filter data."
        )

@router.post("/preview", response_model=ContactExportPreviewResponse)
async def preview_contact_export(req: ContactExportRequest):
    try:
        preview_data = await services.generate_export_preview(req)
        return preview_data
    except Exception as e:
        logger.error(f"Error during contact export preview: {e}", exc_info=True)
        raise HTTPException(status_code=500, detail="Failed to generate preview.")

@router.post("/download")
async def download_contact_export(req: ContactExportRequest):
    try:
        csv_content = await services.generate_export_file_content(req)
        file_name = req.file_name if req.file_name.endswith('.csv') else f"{req.file_name}.csv"
        return StreamingResponse(
            iter([csv_content]),
            media_type="text/csv",
            headers={"Content-Disposition": f"attachment; filename={file_name}"}
        )
    except Exception as e:
        logger.error(f"Error during contact export download: {e}", exc_info=True)
        raise HTTPException(status_code=500, detail="Failed to generate file for download.")

@router.post("/clear-cache")
async def clear_caches():
    """Clear all phone filter related caches."""
    try:
        cache.clear_export_cache()
        cache.clear_engaged_numbers_cache()
        return {"success": True, "message": "All phone filter caches cleared successfully."}
    except Exception as e:
        logger.error(f"Error clearing caches: {e}", exc_info=True)
        raise HTTPException(status_code=500, detail="Failed to clear caches.")

@router.post("/warmup-cache")
async def warmup_cache(background_tasks: BackgroundTasks):
    """Pre-computes and caches results for common export scenarios in the background."""
    background_tasks.add_task(services.warmup_cache_service)
    return {"success": True, "message": "Warming cache for common scenarios in the background."}
```

### 11. Update Phone Filter Services (`backend_python/app/routes/phone_filter/services.py`)

Updated to fetch numbers from both people and entities (Companies/Business Names) if filters allow.

```python
# backend_python/app/routes/phone_filter/services.py
import logging
import random
import csv
import io
import glob
from typing import List, Dict, Any, Set, Optional
from datetime import datetime
from ...elasticsearch_client import es_client
from ...utils.helpers import build_es_query, calculate_age_range, format_standard_number
from ...models import ContactExportRequest, ContactExportPreviewStats, ContactExportPreviewResponse
from . import cache, utils

logger = logging.getLogger(__name__)

def get_engaged_numbers() -> Set[str]:
    """
    Loads already-engaged numbers from CSV files. Caches the result to avoid excessive file I/O.
    """
    if cache._cache_last_updated and (datetime.now() - cache._cache_last_updated) < cache.ENGAGED_CACHE_TTL:
        return cache._engaged_numbers_cache

    logger.info("Refreshing already-engaged numbers cache...")
    engaged_numbers = set()
    engaged_path = '/app/main_jsons/already_engaged/*.csv'
    
    for file_path in glob.glob(engaged_path):
        try:
            with open(file_path, 'r', encoding='utf-8') as f:
                reader = csv.DictReader(f)
                for row in reader:
                    phone_key = next((k for k in row.keys() if k.lower().replace(" ", "") == "phonenumber"), None)
                    if phone_key and row[phone_key]:
                        engaged_numbers.add(row[phone_key])
        except Exception as e:
             logger.error(f"Error reading engaged numbers file {file_path}: {e}")

    cache._engaged_numbers_cache = engaged_numbers
    cache._cache_last_updated = datetime.now()
    logger.info(f"Cache refreshed. Loaded {len(cache._engaged_numbers_cache)} engaged numbers.")
    return cache._engaged_numbers_cache

async def fetch_filtered_standard_numbers_from_es(req: ContactExportRequest) -> List[str]:
    """
    Uses Elasticsearch aggregations to efficiently fetch all unique, normalized phone numbers matching the filter criteria.
    Fetches from People AND Companies/BusinessNames if filters (age/nationality) are not restrictive.
    """
    all_numbers = set()
    
    # --- 1. Fetch from People ---
    # People always have details.mobilePhoneNumber_normalized (if phone exists)
    must_conditions_people = [{"exists": {"field": "details.mobilePhoneNumber"}}]
    
    if req.nationality:
         must_conditions_people.append({"term": {"nationality.keyword": req.nationality}})
    
    if req.min_age is not None or req.max_age is not None: # Allow 0 as valid int
        age_range = calculate_age_range(req.min_age, req.max_age)
        if age_range:
            must_conditions_people.append({"range": {"date_of_birth": age_range}})
            
    query_people = {"bool": {"must": must_conditions_people}}
    
    # Aggregation loop for People
    after_key = None
    while True:
        agg_body = {
            "size": 0,
            "query": query_people,
            "aggs": {
                "unique_numbers": {
                    "composite": {
                        "size": 10000,
                        "sources": [{"phone": {"terms": {"field": "details.mobilePhoneNumber_normalized"}}}]
                    }
                }
            }
        }
        if after_key:
            agg_body["aggs"]["unique_numbers"]["composite"]["after"] = after_key
            
        response = await es_client.async_client.search(index="people", body=agg_body)
        buckets = response["aggregations"]["unique_numbers"]["buckets"]
        
        if not buckets:
            break
            
        for bucket in buckets:
            normalized = bucket["key"]["phone"]
            if normalized:
                standard_num = format_standard_number(normalized)
                if standard_num:
                    all_numbers.add(standard_num)
                    
        after_key = response["aggregations"]["unique_numbers"].get("after_key")
        if not after_key:
            break

    # --- 2. Fetch from Entities (Companies/BNs) ---
    # Only if age/nationality filters are NOT applied, as entities don't have these fields.
    # If filters are null/empty, we include entities.
    filters_active = req.nationality is not None or req.min_age is not None or req.max_age is not None
    
    if not filters_active:
        # Entities might not have a normalized field indexed by migration scripts yet for business_place.
        # We might need to aggregate on the keyword field and normalize in python if the dataset isn't huge, 
        # OR trust the keyword is clean enough.
        # Based on elasticsearch_client.py update, we have 'business_place.mobilePhoneNumberBsness.keyword'.
        
        entity_indices = "companies,business_names"
        # We query for both common field names
        should_entity = [
             {"exists": {"field": "business_place.mobilePhoneNumberBsness"}},
             {"exists": {"field": "business_place.mobilePhoneNumberCmpny"}}
        ]
        query_entity = {"bool": {"should": should_entity, "minimum_should_match": 1}}
        
        # Aggregation loop for Entities (Bsness field)
        after_key = None
        while True:
            agg_body = {
                "size": 0,
                "query": query_entity,
                "aggs": {
                    "unique_numbers": {
                        "composite": {
                            "size": 10000,
                            "sources": [{"phone": {"terms": {"field": "business_place.mobilePhoneNumberBsness.keyword"}}}]
                        }
                    }
                }
            }
            if after_key:
                agg_body["aggs"]["unique_numbers"]["composite"]["after"] = after_key
                
            response = await es_client.async_client.search(index=entity_indices, body=agg_body)
            buckets = response["aggregations"]["unique_numbers"]["buckets"]
            if not buckets: break
            for bucket in buckets:
                raw_num = bucket["key"]["phone"]
                # We must normalize here since it might not be pre-normalized
                from ...utils.helpers import normalize_phone_number
                normalized = normalize_phone_number(raw_num)
                if normalized:
                    standard_num = format_standard_number(normalized)
                    if standard_num:
                        all_numbers.add(standard_num)
            after_key = response["aggregations"]["unique_numbers"].get("after_key")
            if not after_key: break
            
        # Aggregation loop for Entities (Cmpny field) - companies sometimes use this key
        after_key = None
        while True:
             agg_body = {
                "size": 0,
                "query": query_entity,
                "aggs": {
                    "unique_numbers": {
                        "composite": {
                            "size": 10000,
                             "sources": [{"phone": {"terms": {"field": "business_place.mobilePhoneNumberCmpny.keyword"}}}]
                        }
                    }
                }
             }
             if after_key: agg_body["aggs"]["unique_numbers"]["composite"]["after"] = after_key
             response = await es_client.async_client.search(index=entity_indices, body=agg_body)
             buckets = response["aggregations"]["unique_numbers"]["buckets"]
             if not buckets: break
             for bucket in buckets:
                 raw_num = bucket["key"]["phone"]
                 from ...utils.helpers import normalize_phone_number
                 normalized = normalize_phone_number(raw_num)
                 if normalized:
                     standard_num = format_standard_number(normalized)
                     if standard_num:
                         all_numbers.add(standard_num)
             after_key = response["aggregations"]["unique_numbers"].get("after_key")
             if not after_key: break

    return list(all_numbers)

async def get_exportable_numbers(req: ContactExportRequest) -> List[str]:
    """
    Gets the final list of exportable numbers by fetching from ES (with caching) and filtering out already engaged numbers.
    """
    cache_key = cache.get_cache_key(req)

    # Check cache first
    if cache_key in cache._export_cache:
        cached_numbers, cached_time = cache._export_cache[cache_key]
        if datetime.now() - cached_time < cache.EXPORT_CACHE_TTL:
            logger.info(f"Using cached results for {cache_key}")
            return cached_numbers
    else:
        # Ensure we don't just delete if key doesn't exist
        if cache_key in cache._export_cache:
            del cache._export_cache[cache_key]
            
    logger.info(f"Computing fresh results for {cache_key}")
    
    all_numbers = await fetch_filtered_standard_numbers_from_es(req)
    unique_numbers = set(all_numbers)
    
    engaged_numbers = get_engaged_numbers()
    
    final_numbers_list = list(unique_numbers - engaged_numbers)
    
    cache._export_cache[cache_key] = (final_numbers_list, datetime.now())
    
    return final_numbers_list

async def generate_export_preview(req: ContactExportRequest) -> ContactExportPreviewResponse:
    """Generates a preview of the export data, including stats and samples."""
    
    # This raw fetch is just for stats
    all_numbers_for_stats = await fetch_filtered_standard_numbers_from_es(req)
    initial_count = len(all_numbers_for_stats)
    
    # Set logic handles uniqueness
    unique_numbers = set(all_numbers_for_stats)
    duplicates_removed = initial_count - len(unique_numbers)
    
    # Get exportable (filtered against engaged)
    final_numbers_list = await get_exportable_numbers(req)
    final_count = len(final_numbers_list)
    
    engaged_removed = len(unique_numbers) - final_count
    
    actual_sample_size = min(req.sample_size, final_count)
    sampled_numbers = random.sample(final_numbers_list, actual_sample_size) if actual_sample_size > 0 else []
    
    sample_data_head = [{"Name": f"{req.name_prefix}{i+1}", "Phone Number": num} for i, num in enumerate(sampled_numbers[:5])]
    
    sample_data_tail = []
    if actual_sample_size > 5:
        tail_start_index = max(5, actual_sample_size - 5)
        sample_data_tail = [{"Name": f"{req.name_prefix}{tail_start_index + i + 1}", "Phone Number": num} for i, num in enumerate(sampled_numbers[tail_start_index:])]
    
    stats = ContactExportPreviewStats(
        initial_count=initial_count,
        duplicates_removed=duplicates_removed,
        engaged_removed=engaged_removed,
        final_count=final_count,
        sample_size=actual_sample_size
    )
    
    return ContactExportPreviewResponse(
        stats=stats,
        sample_data_head=sample_data_head,
        sample_data_tail=sample_data_tail
    )

async def generate_export_file_content(req: ContactExportRequest) -> str:
    """Generates the full CSV file content as a string."""
    final_numbers_list = await get_exportable_numbers(req)
    
    actual_sample_size = min(req.sample_size, len(final_numbers_list))
    sampled_numbers = random.sample(final_numbers_list, actual_sample_size) if actual_sample_size > 0 else []
    
    output = io.StringIO()
    writer = csv.DictWriter(output, fieldnames=['Name', 'Phone Number'])
    writer.writeheader()
    
    for i, number in enumerate(sampled_numbers):
        writer.writerow({'Name': f'{req.name_prefix}{i+1}', 'Phone Number': number})
        
    return output.getvalue()

async def warmup_cache_service():
    """Pre-computes and caches results for common export scenarios."""
    common_scenarios = [
        ContactExportRequest(nationality="Tanzanian", min_age=25, max_age=55, sample_size=5500),
        ContactExportRequest(nationality="Tanzanian", min_age=18, max_age=35, sample_size=5500),
        ContactExportRequest(nationality="Kenyan", min_age=25, max_age=55, sample_size=5500),
        # Add scenario for entity export (no filters)
        ContactExportRequest(nationality=None, min_age=None, max_age=None, sample_size=5500)
    ]
    
    for scenario in common_scenarios:
        try:
            await get_exportable_numbers(scenario)
            logger.info(f"Cache warmed for {cache.get_cache_key(scenario)}")
        except Exception as e:
            logger.error(f"Error warming cache for scenario {scenario}: {e}")
```

### 12. Update Models (`backend_python/app/models.py`)

I need to ensure `ContactExportRequest` allows optional integers for age/nationality logic, as `src/components/phone_filter/ContactExporter.tsx` sends `null` when filters are disabled.

```python
# backend_python/app/models.py
from pydantic import BaseModel, Field
from typing import Optional, List, Dict, Any

# --- Common Models ---
class PaginationResponse(BaseModel):
    current_page: int = Field(..., alias="currentPage")
    total_pages: int = Field(..., alias="totalPages")
    total_count: int = Field(..., alias="totalCount")
    limit: int
    search_after: Optional[str] = Field(None, alias="searchAfter")
    has_more: bool = Field(..., alias="hasMore")

class ErrorResponse(BaseModel):
    success: bool = False
    message: str

# --- Place of Business Models ---
class PlaceItem(BaseModel):
    id: str
    place_name: str = Field(..., alias="placeName")
    place_type: str = Field(..., alias="placeType")
    count: int

class PlacesListResponse(BaseModel):
    success: bool = True
    places: List[PlaceItem]
    pagination: Dict[str, Any]

# --- Company Models ---
class CompanyInfo(BaseModel):
    company_name: str = Field(..., alias="companyName")
    tin: Optional[str] = None
    authorised_share: Optional[str] = Field(None, alias="authorisedShare")
    incorporation_number: Optional[str] = Field(None, alias="incorporationNumber")

class ApplicantInfo(BaseModel):
    approval_status: str = Field(..., alias="approvalStatus")
    time_application_created: str = Field("", alias="timeApplicationCreated")

class CompanyListItem(BaseModel):
    id: str
    registration_type: str = Field("company", alias="registrationType")
    company_info_obj: CompanyInfo = Field(..., alias="companyInfoObj")
    applicant_obj: ApplicantInfo = Field(..., alias="applicantObj")
    highlight: Optional[Dict[str, List[str]]] = None

class CompaniesListResponse(BaseModel):
    success: bool = True
    companies: List[CompanyListItem]
    pagination: PaginationResponse

class CompanyDetailResponse(BaseModel):
    success: bool = True
    company: Dict[str, Any]

# --- Business Name Models ---
class BusinessNameInfo(BaseModel):
    business_name: str = Field(..., alias="businessName")
    registration_number: Optional[str] = Field(None, alias="registrationNumber")
    business_type: Optional[str] = Field(None, alias="businessType")

class BusinessNameListItem(BaseModel):
    id: str
    registration_type: str = Field("business_name", alias="registrationType")
    business_info_obj: BusinessNameInfo = Field(..., alias="businessInfoObj")
    applicant_obj: ApplicantInfo = Field(..., alias="applicantObj")
    highlight: Optional[Dict[str, List[str]]] = None

class BusinessNamesListResponse(BaseModel):
    success: bool = True
    business_names: List[BusinessNameListItem]
    pagination: PaginationResponse

class BusinessNameDetailResponse(BaseModel):
    success: bool = True
    business_name: Dict[str, Any]

# --- Corporate Shareholder Models ---
class CorporateShareholderListItem(BaseModel):
    id: str
    name: str
    country: Optional[str] = None
    shareholding_count: int = Field(..., alias="shareholdingCount")
    highlight: Optional[Dict[str, List[str]]] = None

class CorporateShareholdersListResponse(BaseModel):
    success: bool = True
    corporate_shareholders: List[CorporateShareholderListItem] = Field(..., alias="corporateShareholders")
    pagination: PaginationResponse

class Shareholding(BaseModel):
    company_id: str = Field(..., alias="companyId")
    company_name: str = Field(..., alias="companyName")
    shares_class: str = Field(..., alias="sharesClass")
    shares_count: int = Field(..., alias="sharesCount")
    approval_status: Optional[str] = Field(None, alias="approvalStatus")

class CorporateShareholderDetail(BaseModel):
    id: str
    name: str
    country: Optional[str] = None
    email_address: Optional[str] = Field(None, alias="emailAddress")
    phone_number: Optional[str] = Field(None, alias="phoneNumber")
    shareholdings: List[Shareholding]

class CorporateShareholderDetailResponse(BaseModel):
    success: bool = True
    corporate_shareholder: CorporateShareholderDetail = Field(..., alias="corporateShareholder")

# --- Unified People & Search Models ---
class PersonListItem(BaseModel):
    id: str
    full_name: str = Field(..., alias="fullName")
    date_of_birth: Optional[str] = Field(None, alias="dateOfBirth")
    nationality: Optional[str] = None
    role: str
    registration_id: str = Field(..., alias="registrationId")
    source_record_id: str = Field(..., alias="sourceRecordId")
    registration_name: str = Field(..., alias="registrationName")
    registration_type: str = Field(..., alias="registrationType")
    highlight: Optional[Dict[str, List[str]]] = None

class PeopleListResponse(BaseModel):
    success: bool = True
    people: List[PersonListItem]
    pagination: PaginationResponse

class PersonDetails(BaseModel):
    full_name: str = Field(..., alias="fullName")
    date_of_birth: Optional[str] = Field(None, alias="dateOfBirth")
    nationality: Optional[str] = None

class PersonRole(BaseModel):
    role: str # Can be comma-separated list of roles
    registration_id: str = Field(..., alias="registrationId")
    source_record_id: str = Field(..., alias="sourceRecordId")
    registration_name: str = Field(..., alias="registrationName")
    registration_type: str = Field(..., alias="registrationType")
    approval_status: Optional[str] = Field(None, alias="approvalStatus")

class ShareholdingInfo(BaseModel):
    company_id: str = Field(..., alias="companyId")
    company_name: str = Field(..., alias="companyName")
    share_value: float = Field(..., alias="shareValue")

class AssociatedTin(BaseModel):
    id: str
    tin_no: str = Field(..., alias="tinNo")
    name: Optional[str] = None
    tin_type: Optional[str] = Field(None, alias="tinType")
    postal_city: Optional[str] = Field(None, alias="postalCity")
    business_start_date: Optional[str] = Field(None, alias="businessStartDate")
    telephones: List[str] = []

class PersonProfileDetails(BaseModel):
    emailAddress: Optional[List[str]] = None
    mobilePhoneNumber: Optional[List[str]] = None
    class Config:
        extra = 'allow'

class FullPersonProfile(BaseModel):
    details: PersonDetails
    roles: List[PersonRole]
    detailed_info: Optional[PersonProfileDetails] = Field(None, alias="detailedInfo")
    shareholdings: Optional[List[ShareholdingInfo]] = None
    associated_tins: Optional[List[AssociatedTin]] = Field(None, alias="associatedTins")

class PersonDetailResponse(BaseModel):
    success: bool = True
    person: FullPersonProfile

class CompanyContactResult(BaseModel):
    company_id: str = Field(..., alias="companyId")
    company_name: str = Field(..., alias="companyName")
    found_in_sections: str = Field(..., alias="foundInSections")
    registration_type: str = Field("company", alias="registrationType")
    approval_status: Optional[str] = Field(None, alias="approvalStatus")

class BusinessNameContactResult(BaseModel):
    business_name_id: str = Field(..., alias="businessNameId")
    business_name: str = Field(..., alias="businessName")
    found_in_sections: str = Field(..., alias="foundInSections")
    registration_type: str = Field("business_name", alias="registrationType")
    approval_status: Optional[str] = Field(None, alias="approvalStatus")

class PersonContactResult(BaseModel):
    person_id: str = Field(..., alias="personId")
    full_name: str = Field(..., alias="fullName")
    role: str
    registration_id: str = Field(..., alias="registrationId")
    registration_name: str = Field(..., alias="registrationName")
    registration_type: str = Field(..., alias="registrationType")
    approval_status: Optional[str] = Field(None, alias="approvalStatus")

class ContactSearchResponse(BaseModel):
    success: bool = True
    companies: List[CompanyContactResult]
    business_names: List[BusinessNameContactResult]
    people: List[PersonContactResult]
    associated_tins: List[AssociatedTin] = Field([], alias="associatedTins")

# --- Network Graph Models ---
class GraphNode(BaseModel):
    id: str
    name: str
    type: str 
    val: Optional[float] = 1 
    most_common_role: Optional[str] = Field(None, alias="mostCommonRole")
    date_of_birth: Optional[str] = Field(None, alias="dateOfBirth")
    gender: Optional[str] = None

class GraphLink(BaseModel):
    source: str
    target: str
    roles: List[str]

class GraphData(BaseModel):
    nodes: List[GraphNode]
    links: List[GraphLink]

class GraphDataResponse(BaseModel):
    success: bool = True
    graph: GraphData
    message: Optional[str] = None

# --- Key Influencers Models ---
class LeaderboardPerson(BaseModel):
    id: str
    name: str
    value: float
    nationality: Optional[str] = None

class LeaderboardResponse(BaseModel):
    success: bool = True
    leaderboard: List[LeaderboardPerson]
    pagination: PaginationResponse

class CoDirectorMatrixEntry(BaseModel):
    person1_id: str
    person1_name: str
    person2_id: str
    person2_name: str
    count: int

class CoDirectorMatrixResponse(BaseModel):
    success: bool = True
    matrix: List[CoDirectorMatrixEntry]
    people: List[Dict[str, str]]

class PathNode(BaseModel):
    id: str
    name: str
    type: str

class PathLink(BaseModel):
    source: str
    target: str
    label: str

class SixDegreesResponse(BaseModel):
    success: bool = True
    path_found: bool
    message: str
    nodes: Optional[List[PathNode]] = None
    links: Optional[List[PathLink]] = None

# --- Phone Filter Models ---
class PhoneFilterPerson(BaseModel):
    id: str
    full_name: str = Field(..., alias="fullName")
    age: Optional[int] = None
    mobile_phone_number: Optional[str] = Field(None, alias="mobilePhoneNumber")
    nationality: Optional[str] = None
    formatted_number: Optional[str] = Field(..., alias="formattedNumber")
    standard_number: Optional[str] = Field(..., alias="standardNumber")
    highlight: Optional[Dict[str, List[str]]] = None

class PhoneFilterListResponse(BaseModel):
    success: bool = True
    people: List[PhoneFilterPerson]
    pagination: PaginationResponse

# --- NEW: Contact Exporter Models ---
class ContactExportRequest(BaseModel):
    nationality: Optional[str] = "Tanzanian" # Made optional
    min_age: Optional[int] = 25              # Made optional
    max_age: Optional[int] = 55              # Made optional
    sample_size: int = 5500
    name_prefix: str = "wapp_logo_CM21_"
    file_name: str = "contacts.csv"

class TraContactExportRequest(BaseModel):
    min_age: Optional[int] = None
    max_age: Optional[int] = None
    sample_size: int = 5500
    name_prefix: str = "wapp_logo_CM21_"
    file_name: str = "tins_contacts.csv"

class ContactExportPreviewStats(BaseModel):
    initial_count: int
    duplicates_removed: int
    engaged_removed: int
    final_count: int
    sample_size: int

class ContactExportPreviewResponse(BaseModel):
    success: bool = True
    stats: ContactExportPreviewStats
    sample_data_head: List[Dict[str, str]]
    sample_data_tail: List[Dict[str, str]]

# --- TRA TINS Models ---
class TraTinListItem(BaseModel):
    id: str
    tin_no: str = Field(..., alias="tinNo")
    name: Optional[str] = None
    tin_type: Optional[str] = Field(None, alias="tinType")
    postal_city: Optional[str] = Field(None, alias="postalCity")
    business_start_date: Optional[str] = Field(None, alias="businessStartDate")
    telephones: List[str] = []
    age: Optional[int] = None
    highlight: Optional[Dict[str, List[str]]] = None

class TraTinsListResponse(BaseModel):
    success: bool = True
    tins: List[TraTinListItem]
    pagination: PaginationResponse

class TraTinDetailResponse(BaseModel):
    success: bool = True
    tin: Dict[str, Any]

# --- Wabunge Models ---
class WabungeListItem(BaseModel):
    id: str
    name: str
    title: Optional[str] = None
    phone1: Optional[str] = None
    phone2: Optional[str] = None
    chama: Optional[str] = None
    address1: Optional[str] = None
    address2: Optional[str] = None
    highlight: Optional[Dict[str, List[str]]] = None

class WabungeListResponse(BaseModel):
    success: bool = True
    wabunge: List[WabungeListItem]
    pagination: PaginationResponse

class WabungeDetailResponse(BaseModel):
    success: bool = True
    mbunge: WabungeListItem
```
